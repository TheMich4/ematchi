<script lang="ts">
	import Game from './game.svelte';
	import '../styles.css';
	import Modal from './modal.svelte';
	import { levels } from './levels';
	import { confetti } from '@neoconfetti/svelte';
	import Button from './button.svelte';

	let state: 'waiting' | 'playing' | 'paused' | 'won' | 'lost' = 'waiting';

	let game: Game;
</script>

<Game
	bind:this={game}
	on:play={() => {
		state = 'playing';
	}}
	on:pause={() => {
		state = 'paused';
	}}
	on:win={() => {
		state = 'won';
	}}
	on:lose={() => {
		state = 'lost';
	}}
/>

{#if state !== 'playing'}
	<Modal>
		<div class="flex flex-col items-center">
			<header class="mb-8 flex flex-col items-center">
				<h1 class="text-8xl">
					e<span class="text-purple-700">match</span>i
				</h1>
				<p>the emoji matching game</p>
			</header>

			<div class="mb-2">
				{#if state === 'won' || state === 'lost'}
					<p>you {state} the game!</p>
				{:else if state === 'paused'}
					<p>game paused</p>
				{:else if state === 'waiting'}
					<p>choose a level:</p>
				{/if}
			</div>

			<div class="flex flex-row gap-2">
				{#if state === 'paused'}
					<Button
						on:click={() => {
							game.resume();
						}}>resume</Button
					>
					<Button
						on:click={() => {
							state = 'waiting';
						}}>quit</Button
					>
				{:else}
					{#each levels as level}
						<Button
							on:click={() => {
								game.start(level);
							}}>{level.label}</Button
						>
					{/each}
				{/if}
			</div>
		</div>
	</Modal>
{/if}

{#if state === 'won'}
	<div
		class="!fixed !h-full !w-full left-[50%] top-[30%] pointer-events-none"
		use:confetti={{ stageWidth: innerWidth, stageHeight: innerHeight }}
	/>
{/if}

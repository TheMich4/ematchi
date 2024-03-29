<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import Countdown from './countdown.svelte';
	import Found from './found.svelte';
	import Grid from './grid.svelte';
	import { levels, type Level } from './levels';
	import { shuffle } from './utils';

	const level = levels[0];

	const dispatch = createEventDispatcher();

	let size: number;
	let grid: string[] = [];
	let found: string[] = [];
	let remaining: number = 0;
	let duration: number = 0;
	let playing: boolean = false;

	export function start(level: Level) {
		found = [];
		size = level.size;
		grid = createGrid(level);
		remaining = duration = level.duration;

		resume();
	}

	export function resume() {
		playing = true;
		countdown();

		dispatch('play');
	}

	function createGrid(level: Level) {
		const copy = level.emojis.slice();
		const pairs: string[] = [];

		for (let i = 0; i < level.size ** 2 / 2; i++) {
			const index = Math.floor(Math.random() * copy.length);
			const emoji = copy[index];

			copy.splice(index, 1);
			pairs.push(emoji);
		}

		pairs.push(...pairs);

		return shuffle(pairs);
	}

	function countdown() {
		const start = Date.now();
		let remainingAtStart = remaining;

		function loop() {
			if (!playing) return;

			requestAnimationFrame(loop);

			remaining = remainingAtStart - (Date.now() - start);
			if (remaining <= 0) {
				dispatch('lose');
				playing = false;
			}
		}

		loop();
	}
</script>

<div class="game flex flex-col justify-center items-center h-full gap-4" style="--size: {size}">
	<div class="h-fit w-[80em]">
		{#if playing}
			<Countdown
				{remaining}
				{duration}
				on:click={() => {
					playing = false;
					dispatch('pause');
				}}
			/>
		{/if}
	</div>

	<div class="w-[80em] h-[80em]">
		<Grid
			{grid}
			on:found={(e) => {
				found = [...found, e.detail.emoji];

				if (found.length === (size * size) / 2) {
					dispatch('win');
				}
			}}
			{found}
		/>
	</div>

	<div class="h-fit w-[80em]">
		<Found {found} />
	</div>
</div>

<style>
	.game {
		font-size: min(1vmin, 0.5rem);
	}
</style>

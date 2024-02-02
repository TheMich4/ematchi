<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import Square from './square.svelte';

	export let grid: string[];
	export let found: string[];

	const dispatch = createEventDispatcher();

	let a: number = -1;
	let b: number = -1;
	let resetTimeout: number;
</script>

<div class="grid">
	{#each grid as emoji, i}
		<Square
			{emoji}
			on:click={() => {
				clearTimeout(resetTimeout);

				if (a === -1 && b === -1) {
					a = i;
				} else if (b === -1) {
					b = i;

					if (grid[a] === grid[b]) {
						dispatch('found', { emoji });
					} else {
						resetTimeout = setTimeout(() => {
							a = b = -1;
						}, 1000);
					}
				} else {
					b = -1;
					a = i;
				}
			}}
			selected={a === i || b === i}
			found={found.includes(emoji)}
			group={grid.indexOf(emoji) === i ? 'a' : 'b'}
		/>
	{/each}
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		grid-template-rows: repeat(4, 1fr);
		grid-gap: 0.5em;
		height: 100%;
		perspective: 100vw;
	}
</style>

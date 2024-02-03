<script lang="ts">
	import { send } from './transitions';
	import { getTwemojiUrl } from './utils';

	export let emoji: string;
	export let selected: boolean;
	export let found: boolean;
	export let group: 'a' | 'b';
</script>

<div class="square flex justify-center items-center" class:flipped={selected || found}>
	<button on:click class="absolute size-full border-0 rounded-lg bg-zinc-500" />

	<div class="background bg-zinc-900 border-4 border-purple-700 size-full absolute rounded-lg" />

	{#if !found}
		<img
			alt={emoji}
			src={getTwemojiUrl(emoji)}
			out:send={{ key: `${emoji}:${group}` }}
			class="size-12 max-width-[80%] pointer-events-none"
		/>
	{/if}
</div>

<style>
	.square {
		transform-style: preserve-3d;
		transition: transform 0.4s;
	}

	.flipped {
		transform: rotateY(180deg);
	}

	button {
		backface-visibility: hidden;
		font-size: inherit;
	}

	.background {
		transform: rotateY(180deg);
		backface-visibility: hidden;
	}

	img {
		transform: rotateY(180deg);
		backface-visibility: hidden;
	}
</style>

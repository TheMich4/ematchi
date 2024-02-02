<script lang="ts">
	import { receive } from './transitions';
	import { getTwemojiUrl } from './utils';

	export let found: string[] = [];
</script>

<div class="found flex justify-center itesm-center gap-2 h-full items-center">
	{#each found as emoji}
		<div
			class="flex justify-center items-center size-12 bg-zinc-900 border-2 border-purple-700 rounded-md"
		>
			<img alt={emoji} src={getTwemojiUrl(emoji)} in:receive={{ key: `${emoji}:a` }} />
			<img alt={emoji} src={getTwemojiUrl(emoji)} in:receive={{ key: `${emoji}:b` }} />
		</div>
	{/each}
</div>

<style>
	.found {
		filter: drop-shadow(0.2em 0.2em 0.5em rgba(0, 0, 0, 0.2));
	}

	img {
		position: absolute;
		width: 4em;
		height: 4em;
	}
</style>

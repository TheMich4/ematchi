<script lang="ts">
	import { getTwemojiUrl } from './utils';

	export let found: string[] = [];
</script>

<div class="found">
	{#each found as emoji}
		<img alt={emoji} src={getTwemojiUrl(emoji)} />
	{/each}
</div>

<style>
	.found {
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 0.5em;
		height: 100%;
	}

	img {
		width: 6em;
		height: 6em;
	}
</style>

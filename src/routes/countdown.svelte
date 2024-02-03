<script lang="ts">
	export let remaining: number;
	export let duration: number;
</script>

<div class="flex items-center gap-4">
	<button on:click class="size-12 sm:size-14 md:size-16 bg-transparent border-0 text-zinc-500">
		<svg
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 24 24"
			class="absolute size-full left-0 top-0"
			><title>pause-circle</title>
			<path
				d="M15,16H13V8H15M11,16H9V8H11M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2Z"
				fill="#999"
			/></svg
		>
	</button>

	<div class="flex-1 h-2 bg-zinc-500 w-full rounded-md">
		<div class="remaining absolute h-full bg-purple-700" style="--p: {remaining / duration}" />
	</div>
</div>

<style>
	.remaining {
		width: calc(100% * var(--p));
	}
</style>

<script>
	export let mode;
	export let reduceMotion;
</script>

<main>
	{#if reduceMotion && mode == 'stairwell'}
		<div class={mode + '-reduce-motion'} />
	{:else}
		<div class={mode + '-image'} />
	{/if}
</main>

<style>
	main {
		width: 100%;
		padding: 5px;
		display: flex;
		justify-content: center;
	}
	div {
		background-position: center;
		background-size: 90%;
		background-repeat: no-repeat;
		width: 100%;
		max-width: 384px;
		height: 120px;
	}
	.stairwell-image {
		background-image: url('/animatedlogo_small.gif');
	}
	.stairwell-reduce-motion {
		background-image: url('/stairwell_header.png');
	}
	.purdue-image {
		background-image: url('/purdue_header.png');
	}
</style>

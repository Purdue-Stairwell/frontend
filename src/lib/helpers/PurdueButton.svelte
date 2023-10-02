<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	export let text;
	export let size = 1;
	export let textColor = 'white';
	export let arrow = 'none';

	function clicked() {
		dispatch('click', text);
	}
</script>

<button on:click={clicked} style="--size: {size * 10}px; color: {textColor};">
	{#if arrow === 'back'}
		<img src="/icons/white-back.svg" width="30px" alt="arrow pointing backwards" />
	{:else if arrow === 'next'}
		<img src="/icons/white-next.svg" width="30px" alt="arrow pointing forwards" />
	{/if}
	<span style="grid-column: 2;">{text}</span>
	
	{#if arrow === 'back'}
		<img src="/icons/white-back.svg" width="30px" alt="arrow pointing backwards" />
	{:else if arrow === 'next'}
		<img src="/icons/white-next.svg" width="30px" alt="arrow pointing forwards" />
	{/if}
</button>

<style>
	button {
		padding: var(--size) 2px;
		background: none;
		border: 2px solid rgb(142, 111, 62);
		font-size: 30px;
		font-weight: 400;
		width: 100%;
		margin: 9 auto;
		display: grid;
		place-items: center center;
		grid-template-columns: 1fr 2fr 1fr;
	}

	button:hover,
	button:active {
		background-color: rgb(142, 111, 62);
	}
</style>

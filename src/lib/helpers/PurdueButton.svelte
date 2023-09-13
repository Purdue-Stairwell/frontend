<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let text;

	function clicked() {
		dispatch('click', text);
	}
</script>

<button on:click={clicked}>{text}</button>

<style>
	button {
		padding: 10px 2px;
		background: none;
		border: 2px solid rgb(142, 111, 62);
		font-size: 18px;
		font-weight: 400;
		width: 100%;
	}

	button:hover,
	button:active {
		background-color: rgb(142, 111, 62);
		color: white;
	}
</style>

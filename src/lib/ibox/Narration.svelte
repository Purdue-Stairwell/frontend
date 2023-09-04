<script>
	import { fade, fly } from "svelte/transition";
	import { createEventDispatcher, onMount } from "svelte";

	export let scriptPage;
	export let reduceMotion;

	var i = 0;
	var txt = scriptPage; /* The text */
	var speed = 20; /* The speed/duration of the effect in milliseconds */
	var typed = "";

	function typeWriter() {
		if (i < txt.length) {
			typed += txt.charAt(i);
			i++;
			setTimeout(typeWriter, speed);
		} //....
	}

	onMount(() => {
		if (!reduceMotion) {
			typeWriter();
		}
	});
</script>

<main>
	{#if reduceMotion}
		<p>{scriptPage}</p>
	{:else}
		<p>{typed}</p>
	{/if}
</main>

<style>
	main {
		display: flex;
		justify-content: space-around;
		background-color: #90d7ff;
		align-items: center;
		border-radius: 15px;
		margin: 15px 25px;
		border: 3px solid #ffffff;
		box-shadow: 5px 5px 0px rgba(0, 0, 0, 0.5);
	}
	p {
		line-height: 1.5;
		font-size: 14pt;
		height: 150px;
		background-color: #90d7ff;
		padding: 10px;
		border-radius: 15px;
		font-weight: 700;
		flex: 1;
	}
</style>

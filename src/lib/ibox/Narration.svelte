<script>
	import { createEventDispatcher, onMount } from "svelte";

	const dispatch = createEventDispatcher();

	export let scriptPage;
	export let reduceMotion;

	function typingfinished() {
		dispatch("typingfinished");
	}

	var i = 0;
	var txt = scriptPage; /* The text */
	var speed = 20; /* The speed/duration of the effect in milliseconds */
	var typed = "";
	let pretyped = scriptPage;

	function typeWriter() {
		if (i < txt.length) {
			typed += txt.charAt(i);
			pretyped = pretyped.slice(1);
			i++;
			setTimeout(typeWriter, speed);
		} else {
			typingfinished();
		}
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
		<p>{typed}<span>{pretyped}</span></p>
	{/if}
</main>

<style>
	main {
		display: flex;
		justify-content: space-around;
		background-color: #90d7ff;
		align-items: center;
		border-radius: 15px;
		margin: 0 25px;
		border: 3px solid #ffffff;
		box-shadow: 5px 5px 0px rgba(0, 0, 0, 0.5);
		transition-duration: 0.1s;
	}

	p {
		line-height: 1.4;
		font-size: 16pt;
		height: fit-content;
		background-color: #90d7ff;
		padding: 10px;
		border-radius: 15px;
		font-weight: 700;
		flex: 1;
		transition-duration: 0.1s;
	}
	span {
		color: rgba(0, 0, 0, 0.2);
	}
</style>

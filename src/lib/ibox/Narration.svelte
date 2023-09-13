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
		<span>{scriptPage}</span>
	{:else}
		<span>{typed}<span class="pretyped">{pretyped}</span></span>
	{/if}
</main>

<style>
	main {
		
		display: flex;
		justify-content: space-around;
		align-items: center;
		border-radius: 15px;
		margin: 0 25px;
		/* box-shadow: 5px 5px 0px rgba(0, 0, 0, 0.5); */
		transition-duration: 0.1s;
	}

	span {
		line-height: 1.4;
		color: white;
		font-size: 16pt;
		height: fit-content;
		padding: 10px;
		font-weight: 700;
		flex: 1;
		transition-duration: 0.1s;
		text-shadow: 2px 2px 1px rgba(0, 0, 0, 0.5);
	}
	.pretyped {
		color: rgba(255, 255, 255, 0.2);
	}
</style>

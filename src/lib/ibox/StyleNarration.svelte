<script>
	import { createEventDispatcher, onMount } from "svelte";
	import Typer from "./Typer.svelte";

	const dispatch = createEventDispatcher();

	export let scriptPage;
	export let reduceMotion;

	function typingfinished() {
		dispatch("typingfinished");
	}

	// ten long empty array (dont judge me)
	let typers = [undefined, undefined, undefined,undefined, undefined, undefined, undefined, undefined, undefined, undefined];
	let currentTyper = 0;
	function nextTyper() {
		currentTyper++;
		if(typers[currentTyper] !== undefined) {
			typers[currentTyper].typeWriter();
		}
	}

	onMount(() => {
		if (!reduceMotion) {
			typers[currentTyper].typeWriter();
		}
	});
</script>

<main>
	{#each scriptPage as line, i}
		<Typer bind:this={typers[i]} isTyped={line.typed} style={line.style} text={line.text} on:typingfinished={nextTyper} />
	{/each}
</main>

<style>
	main {
		margin: 0 25px;
		transition-duration: 0.1s;
	}
</style>

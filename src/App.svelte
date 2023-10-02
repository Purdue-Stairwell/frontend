<script>
	import Start from "./lib/Start.svelte";
	import Journey from "./lib/Journey.svelte";
	import End from "./lib/End.svelte";
  	import { onMount } from "svelte";

	let reduceMotion = false;
	let isOver18 = false;
	function updateMotion(event) {
		reduceMotion = event.detail;
	}

	function updateAge(event) {
		isOver18 = event.detail;
	}

	let stage = "end";
	function changeStage(event) {
		stage = event.detail;
	}
</script>
<main>
	{#if stage == "start"}
		<Start {reduceMotion} {isOver18} on:changeStage={changeStage} on:updateMotion={updateMotion} on:updateAge={updateAge} />
	{:else if stage == "journey"}
		<Journey {reduceMotion} {isOver18} on:changeStage={changeStage} />
	{:else if stage == "end"}
		<End {reduceMotion} />
	{/if}
</main>

<style>
	main {
		width: 100vw;
		max-width: 500px;
		padding: 0;
		margin: 0 auto;
		height: 110vh;
	}
</style>

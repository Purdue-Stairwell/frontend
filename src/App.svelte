<script>
	import Start from "./lib/Start.svelte";
	import Journey from "./lib/Journey.svelte";
	import End from "./lib/End.svelte";
  	import { onMount } from "svelte";

	let reduceMotion = false;
	function updateMotion(event) {
		reduceMotion = event.detail;
	}

	let stage = "start";
	function changeStage(event) {
		stage = event.detail;
	}

	onMount(() => {
		setTimeout(function () {
			window.scrollTo(0, 100);
		}, 1000);
	});
</script>
<main>
	{#if stage == "start"}
		<Start {reduceMotion} on:changeStage={changeStage} on:updateMotion={updateMotion} />
	{:else if stage == "journey"}
		<Journey {reduceMotion} on:changeStage={changeStage} />
	{:else if stage == "end"}
		<End {reduceMotion} />
	{/if}
</main>

<style>
	main {
		width: 100%;
		/* max-width: 500px; */
		margin: 0 auto;
		height: 110vh;
	}
</style>

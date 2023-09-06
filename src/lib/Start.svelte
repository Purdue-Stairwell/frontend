<script>
	import { createEventDispatcher } from "svelte";
	import Header from "./Header.svelte";
	import Toggle from "./helpers/Toggle.svelte";
	import { fade } from "svelte/transition";
	import PurdueButton from "./helpers/PurdueButton.svelte";

	const dispatch = createEventDispatcher();

	export let reduceMotion;

	function updateMotion(event) {
		console.log("motion updated: ", event.detail);
		dispatch("updateMotion", event.detail);
	}

	function changeStage() {
		dispatch("changeStage", "journey");
	}
</script>

<main in:fade>
	<!--HEADER-->
	<Header {reduceMotion} mode="purdue" />
	<p>Welcome to StairWELL, an interactive sculpture and gathering place.</p>
	<img src="purdue_stairs.png" alt="Abstract representation of the StairWELL sculpture" />
	<PurdueButton on:click={changeStage} text="Enter!" />
	<div>
		<Toggle on:stateChange={updateMotion} />
		<p>Toggle Reduce Motion</p>
	</div>
</main>

<style>
	* {
		font-family: "Acumin Pro", sans-serif;
	}
	main {
		background-color: #ffffff;
		margin: 0 auto;
		width: 100%;
		height: 100dvh;
		max-width: 500px;
		padding: 0 1rem;
		display: flex;
		justify-content: flex-start;
		flex-flow: column nowrap;
		gap: 0.5rem;
	}

	p {
		text-align: center;
		font-size: 2rem;
	}
	img {
		width: 70%;
		margin: 0 auto;
	}

	div {
		width: 100%;
		display: flex;
		justify-content: space-around;
		gap: 0.5rem;
		padding: 1rem 1rem;
	}

	div p {
		font-size: 1.5rem;
		padding-top: 0.2rem;
	}
</style>

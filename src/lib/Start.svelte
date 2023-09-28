<script>
	import { createEventDispatcher, onMount } from "svelte";
	import Header from "./Header.svelte";
	import Toggle from "./helpers/Toggle.svelte";
	import { fade } from "svelte/transition";
	import PurdueButton from "./helpers/PurdueButton.svelte";

	const dispatch = createEventDispatcher();

	export let reduceMotion = false;

	export let isOver18 = false;

	function updateMotion(event) {
		console.log("motion updated: ", event.detail);
		dispatch("updateMotion", event.detail);
	}

	function updateAge(event) {
		console.log("age updated: ", isOver18);
		dispatch("updateAge", isOver18);
	}

	function changeStage() {
		dispatch("changeStage", "journey");
	}
</script>

<main in:fade>
	<div style="height: 100px"></div>
	<!--HEADER-->
	<Header {reduceMotion} mode="purdue" />
	<p>Welcome to StairWELL, an interactive sculpture and gathering place.</p>
	<img src="purdue_stairs.png" alt="Abstract representation of the StairWELL sculpture" />
	<label>
		Are you over 18? <input type="checkbox" bind:checked={isOver18} on:change={updateAge} />
	</label>
	<PurdueButton on:click={changeStage} text="Enter!" arrow="next" />
	<!-- <div>
		<Toggle on:stateChange={updateMotion} />
		<p>Toggle Reduce Motion</p>
	</div> -->
</main>

<style>
	* {
		font-family: "Acumin Pro", sans-serif;
	}
	main {
		background-color: #000000;
		margin: 0 auto;
		width: 100%;
		height: 100dvh;
		max-width: 500px;
		padding: 0 1rem;
		display: flex;
		justify-content: flex-start;
		flex-flow: column nowrap;
		gap: 0.5rem;
		color: white;
	}

	p {
		text-align: center;
		font-size: 2rem;
	}
	label {
		width: 100%;
		text-align: center;
		font-size: 1.5rem;
	}

	/* checkbox*/
	input[type="checkbox"] {
		height: 1.5rem;
		width: 1.5rem;
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

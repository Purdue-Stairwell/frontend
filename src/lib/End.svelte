<script>
	import Header from "./Header.svelte";
	import { fade } from "svelte/transition";
	import Resources from "./Resources.svelte";
	import PurdueButton from "./helpers/PurdueButton.svelte";
	import Sponsors from "./Sponsors.svelte";
	import Credits from "./Credits.svelte";
	import { onMount } from "svelte";

	export let reduceMotion;
	let endState = "Back";

	function changeEndState(event) {
		console.log("end state changed: ", event.detail);
		endState = event.detail;
	}

	onMount(() => {
		document.documentElement.style.setProperty("background-color", "#fff");
	});
</script>

<main in:fade>
	<!--HEADER-->
	<Header {reduceMotion} mode="purdue" />
	{#if endState === "Back"}
	<p>Purdue has an amazing collection of people and services to help you thrive. Check out this portal…</p>
		<PurdueButton on:click={changeEndState} size={2} text="Resources" />
		<a href="https://it.purdue.edu/services/qualtrics.php"><PurdueButton text="Take a 2 min survey!" /></a>
		<PurdueButton on:click={changeEndState} text="Sponsors" />
		<PurdueButton on:click={changeEndState} text="Credits" />
		<p>Or...</p>
		<PurdueButton
			on:click={(e) => {
				location.reload();
			}}
			text="Restart"
		/>
	{:else if endState === "Resources"}
		<PurdueButton on:click={changeEndState} text="Back" />
		<Resources on:changeState={changeEndState} />
	{:else if endState === "Sponsors"}
		<PurdueButton on:click={changeEndState} text="Back" />
		<Sponsors on:changeState={changeEndState} />
	{:else if endState === "Credits"}
		<PurdueButton on:click={changeEndState} text="Back" />
		<Credits on:changeState={changeEndState} />
	{/if}
</main>

<style>
	* {
		font-family: "Acumin Pro", sans-serif;
	}
	main {
		background-color: #ffffff;
		margin: 0 auto;
		width: 100%;
		max-width: 500px;
		height: 100vh;
		padding: 0 1rem;
		display: flex;
		flex-flow: column nowrap;
		gap: 0.5rem;
	}
	a {
		width: 100%;
	}

	p {
		text-align: center;
		font-size: 1.5rem;
	}
</style>

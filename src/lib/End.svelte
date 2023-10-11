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

		endState = event.detail;
	}
</script>

<main in:fade>
	<!--HEADER-->
	<Header {reduceMotion} mode="purdue" />
	{#if endState === "Back"}
	<p>Purdue has an amazing collection of people and services to help you thrive. Check out this portal…</p>
		<PurdueButton on:click={changeEndState} size={1} text="Resources" />
		<a href="https://purdue.ca1.qualtrics.com/jfe/form/SV_1yJKX4IyLJ3jsl8"><PurdueButton text="Take a 2 min survey!" arrow="next" /></a>
		<PurdueButton on:click={changeEndState} text="Sponsors" />
		<PurdueButton on:click={changeEndState}  text="Credits" />
		<PurdueButton arrow="back"
			on:click={(e) => {
				location.reload();
			}}
			text="Go Again?"
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
		background-color: #000000;
		margin: 0 0.5rem;
		width: calc(100% - 1rem);
		max-width: 500px;
		height: 100%;
		padding: 0;
		display: flex;
		flex-flow: column nowrap;
		align-items: center;
		gap: 0.5rem;
	}
	a {
		width: 100%;
	}

	p {
		text-align: center;
		font-size: 1.5rem;
		color: white;
	}
</style>

<script>
	import { createEventDispatcher, onMount } from "svelte";
	import Header from "./Header.svelte";
	import IRBCheck from "./helpers/IRBCheck.svelte";
	import { fade } from "svelte/transition";
	import PurdueButton from "./helpers/PurdueButton.svelte";

	const dispatch = createEventDispatcher();

	export let reduceMotion = false;

	function updateMotion(event) {
		dispatch("updateMotion", event.detail);
	}

	function updateIRB(event) {
		console.log(event.detail);
		dispatch("updateIRB", event.detail);
	}

	function changeStage() {
		dispatch("changeStage", "journey");
	}
</script>

<main in:fade>
	<!--HEADER-->
	<Header {reduceMotion} mode="purdue" />
	<p class="welcome">An interactive sculpture and gathering place.</p>
	<img src="purdue_stairs.png" alt="Abstract representation of the StairWELL sculpture" />
	<IRBCheck on:updateIRB={updateIRB}/>
	<PurdueButton on:click={changeStage} text="Enter!" arrow="next" />
	
	<p class="irb-blurb">
		Participation in this project is part of a research study. Participation is voluntary.<br/>
		PI: Dr. Carl Krieger - <a href="mailto:ckrieger@purdue.edu">ckrieger@purdue.edu</a><br/>
		Understanding the emotional ecosystem at Purdue on students and the impact of the StairWell project on shaping their perceptions<br/>
		IRB-2023-1434<br/>
		<a href="/">click here to learn how your responses in the app will be used</a>
	</p>
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
		gap: 0.1rem;
		color: white;
	}

	.welcome {
		text-align: center;
		font-size: 1.5rem;
	}

	img {
    	width: 70%;
    	margin: 0 auto;
	}

	.irb-blurb {
		padding-top: 0.5rem;
		text-align: left;
		font-size: 0.75rem;
	}

	.irb-blurb a {
		font-weight: bold;
		color: rgb(142, 111, 62);
	}
</style>

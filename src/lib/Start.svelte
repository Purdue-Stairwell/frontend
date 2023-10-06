<script>
	import { createEventDispatcher, onMount } from "svelte";
	import Header from "./Header.svelte";
	import Toggle from "./helpers/Toggle.svelte";
	import { fade } from "svelte/transition";
	import PurdueButton from "./helpers/PurdueButton.svelte";

	const dispatch = createEventDispatcher();

	export let reduceMotion = false;

	let isOver18;
	let isPurdueStudent;

	function updateMotion(event) {
		dispatch("updateMotion", event.detail);
	}

	function updateAge(event) {
		console.log("age updated: ", isOver18);
		dispatch("updateAge", isOver18 && isPurdueStudent);
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
	<br/>
	<div>
		<p>- Are you a current Purdue Student?</p>
		<label>
			
			<input type="radio" bind:group={isPurdueStudent} value="true" on:change={updateAge} />
			West Lafayette Campus
		</label><br/>
		<label>
			<input type="radio" bind:group={isPurdueStudent} value="false" on:change={updateAge} />
			Purdue Global
		</label><br/>
		<label>
			<input type="radio" bind:group={isPurdueStudent} value="false" on:change={updateAge} />
			Not a Purdue Student
		</label><br/>
		<p>- Are you 18 or older?</p>
		<label>
			<input type="radio" bind:group={isOver18} value="true" on:change={updateAge} />Yes 
		</label>
		<span>    </span>
		<label>
			<input type="radio" bind:group={isOver18} value="false" on:change={updateAge} />No 
		</label>
	</div>
	<br/>
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
	label {
		width: 100%;
		text-align: end;
		font-size: 1.1rem;
	}
	label input {
		margin: 0 0.5rem;
	}
	p {
		font-size: 1.3rem;
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

<script>
	import { fade } from "svelte/transition";
	import { createEventDispatcher } from "svelte";

	const dispatch = createEventDispatcher();

	const options = [
		"At no time",
		"Some of the time",
		"Less than half the time",
		"More than half the time",
		"Most of the time",
		"All of the time",
	];

	export let questionNumber;
	let answer;

	

	function updateAnswer(event) {
		dispatch("whofive", { number: questionNumber, answer: event.target.value });
	}
</script>

<main in:fade>
	{#each [0, 1, 2, 3, 4, 5] as number}
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
		<label >
			<input
				on:click={updateAnswer}
				type="radio"
				name="who5"
				value={number}
				bind:group={answer}
			/>
			<p>{options[number]}</p>
		</label>
	{/each}
</main>

<style>
	main {
		aspect-ratio: 1;
		font-size: 1.6rem;
		display: flex;
		justify-content: space-evenly;
		flex-flow: column nowrap;
		color: #FFFFFF;
		width: 80%;
		margin: 2rem auto;
	}

	input[type="radio"] {
		/* Add if not using autoprefixer */
		-webkit-appearance: none;
		appearance: none;
		/* Not removed via appearance */
		margin: 0 0 0 1rem;
		font: inherit;
		color: #FFFFFF;
		width: 1em;
		height: 1em;
		border: 0.15em solid #FFFFFF;
		border-radius: 50%;
		transform: translateY(0.2em);
		display: grid;
		place-content: center;
	}

	input[type="radio"]::before {
		content: "";
		width: 0.6em;
		height: 0.6em;
		border-radius: 50%;
		transform: scale(0);
		transition: 120ms transform ease-in-out;
		box-shadow: inset 1em 1em #FFFFFF;
	}
	input[type="radio"]:checked::before {
		transform: scale(1);
	}
	label > p {
		transform: translate(3rem, -1.4rem);
		font-size: 1.3rem;
	}
</style>

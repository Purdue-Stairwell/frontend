<script>
	import { fade } from 'svelte/transition';
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	const options = [
		'All of the time',
		'Most of the time',
		'More than half the time',
		'Less than half the time',
		'Some of the time',
		'At no time',
	];

	export let questionNumber;

	let answer;

	function updateAnswer(event) {
		dispatch('whofive', { number: questionNumber, answer: event.target.value });
	}
</script>

<main in:fade>
	{#each [5, 4, 3, 2, 1, 0] as number}
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
		<label on:click={updateAnswer}>
			<input type="radio" name="who5" value={number} bind:group={answer} />
			<p>{options[number]}</p>
		</label>
	{/each}
</main>

<style>
	main {
		margin: 25px;
		aspect-ratio: 1;
		border-radius: 15px;
		background-color: #90d7ff;
		font-size: 1.6rem;
		box-shadow: 3px 3px 2px #000015;
		padding: 0 10px;
		display: flex;
		justify-content: space-evenly;
		border: 3px solid white;
		flex-flow: column nowrap;
	}

	input[type='radio'] {
		/* Add if not using autoprefixer */
		-webkit-appearance: none;
		appearance: none;
		/* Not removed via appearance */
		margin: 0 0 0 1.8rem;
		font: inherit;
		color: #000000;
		width: 1.15em;
		height: 1.15em;
		border: 0.15em solid #000000;
		border-radius: 50%;
		transform: translateY(0.4em);
		display: grid;
		place-content: center;
	}

	input[type='radio']::before {
		content: '';
		width: 0.65em;
		height: 0.65em;
		border-radius: 50%;
		transform: scale(0);
		transition: 120ms transform ease-in-out;
		box-shadow: inset 1em 1em #000000;
	}
	input[type='radio']:checked::before {
		transform: scale(1);
	}
	label > p {
		transform: translate(2.4em, -1em);
	}
</style>

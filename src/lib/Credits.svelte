<script>
	import { fade } from 'svelte/transition';
	import Modal from './helpers/Modal.svelte';

	const teamData = fetch('/jsonData/teamInfo.json').then((res) => res.json());

	let showModal = false;
	let index = 0;
</script>

<main in:fade>
	{#await teamData}
		<h1>Loading StairWELL Team Information...</h1>
	{:then result}
		{#each result.teamData as person, i}
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<div
				class="gallery-unit"
				on:click={() => {
					showModal = true;
					index = i;
				}}
			>
				<img src={person.image} alt={person.name} />
				<h1>{person.name}</h1>
				<p>{person.title}</p>
			</div>
		{/each}
		<Modal bind:showModal>
			<div class="modal-container">
				<img
					src={result.teamData[index].image}
					alt={result.teamData[index].name}
				/>
				<h1>{result.teamData[index].name}</h1>
				<p>{result.teamData[index].title}</p>
				<p>{result.teamData[index].info}</p>
			</div>
		</Modal>
	{/await}
</main>

<style>
	main {
		/* grid of 2 by n */
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		grid-auto-rows: 1fr;
		/* GAP */
		gap: 1rem;
		margin: 0 1rem;
		padding-bottom: 1rem;
	}

	.gallery-unit {
		border: 2px solid rgb(142, 111, 62);
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		padding: 0.8rem;
	}
	.gallery-unit img {
		object-fit: cover;
		max-height: 100px;
		max-width: 100%;
		border: 3px solid rgb(142, 111, 62);
	}
	.gallery-unit h1 {
		font-size: 1rem;
		width: 100%;
		text-align: center;
	}
	.gallery-unit p {
		font-size: 0.8rem;
	}

	.modal-container {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		padding: 0.8rem;
	}
	.modal-container img {
		object-fit: contain;
		max-height: 100px;
		max-width: 100%;
	}
	.modal-container h1 {
		font-size: 1rem;
		width: 100%;
		text-align: center;
		margin-top: 1rem;
	}
	.modal-container p {
		font-size: 0.8rem;
		margin-top: 0.5rem;
	}
</style>

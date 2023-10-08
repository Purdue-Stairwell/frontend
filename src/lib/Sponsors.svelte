<script>
	import { fade } from 'svelte/transition';
	import Modal from './helpers/Modal.svelte';
	import PurdueButton from './helpers/PurdueButton.svelte';

	const resourceInfo = fetch('/jsonData/sponsorInfo.json').then((res) =>
		res.json()
	);

	let showModal = false;
	let index = 0;
</script>

<main in:fade>
	{#await resourceInfo}
		<h1>Loading Resource Info...</h1>
	{:then result}
		{#each result.sponsorInfo as sponsor, i}
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			{#if !showModal}
				<a href={sponsor.link}>
					<button class="sponsor-button">
					<!-- <button
						class="sponsor-button"
						on:click={() => {
							showModal = true;
							index = i;
						}}
					> -->
						<img src={sponsor.image} alt="Logo for {sponsor.name}" />
					</button>
				</a>
			{:else}
				<button class="sponsor-button">
					<img src={sponsor.image} alt="Logo for {sponsor.name}" />
				</button>
			{/if}
		{/each}
		<Modal bind:showModal>
			<div class="modal-container">
				<img
					src={result.sponsorInfo[index].image}
					alt="Logo for {result.sponsorInfo[index].name}"
				/>
				<p>{result.sponsorInfo[index].info}</p>
				<a href={result.sponsorInfo[index].link}
					><PurdueButton text="Website" textColor="black"/></a
				>
			</div>
		</Modal>
	{/await}
</main>

<style>
	/*full width, full height, grid 1 column, 6 rows */
	main {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: repeat(auto, 1fr);
		gap: 0.2rem;
		max-width: 100%;
		padding-bottom: 1rem;
	}

	img {
		object-fit: contain;
		width: 100%;
		aspect-ratio: 1;
	}
	.modal-container {
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
		align-items: center;
		height: 60vh;
	}

	.modal-container > img {
		width: 50vw;
		margin: -3rem 0 -1rem 0;
	}

	.sponsor-button {
		background-color: white;
		margin: 0;
		cursor: pointer;
		padding: 1rem;
		width: 100%;
		height: auto;
		border: 2px solid rgb(142, 111, 62);
	}
</style>

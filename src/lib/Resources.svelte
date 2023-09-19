<script>
	import { fade } from 'svelte/transition';
	import Modal from './helpers/Modal.svelte';
	import PurdueButton from './helpers/PurdueButton.svelte';

	const resourceInfo = fetch('/jsonData/resourceInfo.json').then((res) =>
		res.json()
	);

	let showModal = false;
	let index = 0;
</script>

<main in:fade>
	{#await resourceInfo}
		<h1>Loading Resource Info...</h1>
	{:then result}
		{#each result.resourceInfo as resource, i}
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<div
				class="modal-button"
				on:click={() => {
					showModal = true;
					index = i;
				}}
			>
				<p>{resource.name}</p>
			</div>
		{/each}
		<Modal bind:showModal>
			<div class="modal-container">
				<div class="modal-button">
					<img
						src={result.resourceInfo[index].image}
						alt={result.resourceInfo[index].name}
					/>
				</div>
				<p>{result.resourceInfo[index].info}</p>
				<a href={result.resourceInfo[index].link}
					><PurdueButton text="Website" textColor="black"/></a
				>
			</div>
		</Modal>
	{/await}
</main>

<style>
	.modal-container {
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
		align-items: center;
		height: 60vh;
	}

	.modal-container > * {
		margin: 15px 0px;
	}

	main {
		display: flex;
		justify-content: space-around;
		flex-wrap: wrap;
		height: 75%;
		background-color: #000000;
		color: white;
		width: 100%;
		padding-top: 10px;
	}

	.modal-button {
		width: 40%;
		color: white;
		font-weight: bolder;
		font-size: small;
		padding: 5px;
		border: 2px solid rgb(142, 111, 62);
		display: flex;
		justify-content: center;
		align-items: center;
		text-align: center;

		cursor: pointer;
	}

	.modal-button:hover,
	.modal-button:focus,
	.modal-button:active {
		background-color: rgb(142, 111, 62);
		color: white;
	}

	.modal-container > .modal-button {
		width: 90%;
	}
	img {
		object-fit: contain;
		max-height: 150px;
		max-width: 100%;
	}
</style>

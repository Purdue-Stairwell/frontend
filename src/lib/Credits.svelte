<script>
	import { fade } from 'svelte/transition';
	import Modal from './helpers/Modal.svelte';
	import PurdueButton from './helpers/PurdueButton.svelte';

	const resourceInfo = fetch('/jsonData/creditsInfo.json').then((res) =>
		res.json()
	);

	let showModal = false;
	let index = 0;
</script>

<main in:fade>
	<h1>Built at Purdue by:</h1>
	<div>
		<img src="sponsors/convos.png" alt="Purdue Convocations" />
		<img src="sponsors/hallofmusic.png" alt="Hall of Music Productions" />
	</div>
	<h3>Polytechnic Institute</h3>
	<h3>Chromic Duo</h3>
	<h3>Counseling & Psychological Services</h3>
	<h3>Steps to Leaps</h3>
	<h3>Department of Theatre</h3>
</main>

<style>
	/*full width, full height, grid 1 column, 6 rows */
	main {
		width: 100%;
		height: fit-content;
		color: white;
		
	}
	div {
		display: flex;
		justify-content: space-evenly;
	}

	img {
		object-fit: cover;
		width: 40%;
		background-color: white;
	}

	h1, h3 {
		text-align: center;
	}

	h3 {
		font-size: 1.5em;
	}
</style>

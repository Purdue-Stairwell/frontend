<script>
	import { createEventDispatcher } from "svelte";

	const dispatch = createEventDispatcher();

	export let disabled = false;
	export let showProject = false;

	function goNext() {
		dispatch("next");
	}

	function goBack() {
		dispatch("back");
	}

	function project() {
		dispatch("project");
	}

	let w;
	$: projectFontSize = w / 13 + "px";
</script>

<main bind:clientWidth={w}>
	<button on:click={goBack}>
		<img src="/icons/back.svg" width={projectFontSize} alt="arrow pointing back" />
	</button>
	{#if showProject}
		<button class="project" style="font-size: {projectFontSize};" on:click={project}> Project! </button>
	{:else}
		<div class="spacer" />
	{/if}
	<button on:click={goNext} {disabled}>
		<img src="/icons/next.svg" width={projectFontSize} alt="arrow pointing forwards" />
	</button>
</main>

<style>
	main {
		width: 80%;
		display: flex;
		gap: 0.5rem;
		justify-content: space-between;
		margin: 0 auto;
	}
	button {
		user-select: none;
		border: #ffffff 3px solid;
		border-radius: 15px;
		box-shadow: 5px 5px 0px rgba(0, 0, 0, 0.5);
		background-color: #3ae7c7;
		color: #ffffff;
		transition-duration: 0.1s;
		padding: 0 0.5rem;
		height: 100%;
		max-height: 4rem;
		min-height: 3rem;
		flex: 1;
	}

	img {
		min-width: 2rem;
	}

	button:hover:enabled {
		background-color: #a8ffef;
		color: #000000;
	}

	button:active:enabled {
		color: #000000;
		background-color: #a8ffef;
		transform: translate(5px, 5px);
		box-shadow: 0px 0px;
	}

	button:disabled {
		background-color: #4d756e;
		color: #8f8f8f;
		border-color: #bab9b9;
	}

	.project {
		background-color: #ff8f8f;
		flex: 2;
	}

	.project:hover:enabled {
		background-color: #fcacac;
	}

	.project:active:enabled {
		background-color: #fcacac;
	}

	.project:disabled {
		background-color: #a16666;
	}

	.spacer {
		flex: 2;
	}
</style>

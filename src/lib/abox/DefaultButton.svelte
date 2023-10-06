<script>
	import { createEventDispatcher } from "svelte";

	const dispatch = createEventDispatcher();

	export let disabled = false;
	export let showProject = false;
	export let showSave = false;

	function goNext() {
		dispatch("next");
	}

	function goBack() {
		dispatch("back");
	}

	function project() {
		dispatch("project");
	}

	function save() {
		dispatch("save");
	}

	let w;
	$: projectFontSize = w / 13 + "px";
</script>

<main bind:clientWidth={w}>

	<button on:click={goBack}>
		<img src="/icons/white-back.svg" width={projectFontSize} alt="arrow pointing back" />
	</button>

	{#if showProject}
		<button class="project" style="font-size: {projectFontSize};" on:click={project}>Project!</button>
	{:else if showSave}
		<button class="project" style="font-size: {projectFontSize};" on:click={save}>Share!</button>
	{:else}
		<div class="spacer" />
	{/if}
	{#if !showProject}
		<button on:click={goNext} {disabled} >
			<img src="/icons/white-next.svg" width={projectFontSize} alt="arrow pointing forwards" />
		</button>
	{/if}
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
		border: rgb(142, 111, 62) 3px solid;
		background-color: black;
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
		background-color: rgb(142, 111, 62);
		color: #000000;
	}

	button:active:enabled {
		color: #000000;
		background-color: rgb(142, 111, 62);
		transform: translate(5px, 5px);
		box-shadow: 0px 0px;
	}

	button:disabled {
		background-color: rgb(68, 62, 51);
		color: #383838;
	}

	.project {
		flex: 2;
	}

	.spacer {
		flex: 2;
	}
</style>

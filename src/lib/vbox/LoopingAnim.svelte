<script>
	import { onMount } from "svelte";
	import { fade } from "svelte/transition";
	export let screenLocation;
	export let reduceMotion;
	export let loop = true;

	let journeyState =
		//0     1     2    3    4    5    6   7   8   9   10  11   12   13    14   15-->
		//anim squig animm anim anim anim who who who who who anim anim squig static static-->
		[0, 1, 0, 0, 0, 0, 2, 2, 2, 2, 2, 0, 0, 1, 0, 0];

	let srcs = [
		"/anim/intro_endloop.mp4",
		"SQUIGGLE",
		"/anim/kikithinks.mp4",
		"/anim/goingup.mp4",
		"/anim/barret_idle.mp4",
		"/anim/stair_comp.mp4",
		"WHO",
		"WHO",
		"WHO",
		"WHO",
		"WHO",
		"/anim/all_characters.mp4",
		"/anim/all_characters.mp4",
		"SQUIGGLE",
		"SQUIGGLE",
		"SQUIGGLE",
		"/looping_intro.mp4",
	];

	onMount(() => {
		console.log("looping loaded");
	});
</script>

<main>
	<!-- svelte-ignore a11y-media-has-caption -->
	{#key screenLocation}
		{#if screenLocation < 14}
			<video autoplay={!reduceMotion} muted={reduceMotion} {loop} playsinline>
				<source src={srcs[screenLocation]} type="video/mp4" />
			</video>
		{:else if screenLocation >= 14}
			<img src={srcs[screenLocation]} alt="Narrative Character reading the text" />
		{/if}
	{/key}
</main>

<style>
	main {
		aspect-ratio: 1;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	img {
		width: 100%;
		aspect-ratio: 1;
	}

	video {
		width: 100%;
		aspect-ratio: 1;
		box-shadow: 0px 0px -10px 0px rgba(0, 0, 0, 0.75);
	}
</style>

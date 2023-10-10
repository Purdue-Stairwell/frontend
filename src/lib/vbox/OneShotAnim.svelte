<script>
	import { onMount } from "svelte";
	import { fade } from "svelte/transition";
	export let screenLocation;
	export let reduceMotion;
	export let oneShotEnded = false;

	let journeyState =
		//0     1     2    3    4    5    6   7   8   9   10  11   12   13    14   15-->
		//anim squig animm anim anim anim who who who who who anim anim squig static static-->
		[0, 1, 0, 0, 0, 0, 2, 2, 2, 2, 2, 0, 0, 1, 0, 0];

	let srcs = [
		"/anim/intro.mp4",
		"/anim/user_sketch_light.mp4",
		"/anim/reflection.mp4",
		"/anim/goingup.mp4",
		"/anim/zoom.mp4",
		"/anim/kikithinks.mp4",
		"WHO",
		"WHO",
		"WHO",
		"WHO",
		"WHO",
		"/anim/stair_comp.mp4",
		"/anim/all_characters.mp4",
		"SQUIGGLE",
		"SQUIGGLE",
		"/anim/jump_in.mp4",
	];

	onMount(() => {
		console.log("oneshot loaded");
	});
</script>

<main in:fade>
	<!-- svelte-ignore a11y-media-has-caption -->
	{#key screenLocation}
			<video
				autoplay={!reduceMotion}
				muted={reduceMotion}
				playsinline
				on:ended={() => {
					oneShotEnded = true;
				}}
			>
				<source src={srcs[screenLocation]} type="video/mp4" />
			</video>
	{/key}
</main>

<style>
	main {
		aspect-ratio: 1;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	video {
		width: 100%;
		aspect-ratio: 1;
	}
</style>

<script>
	import { fade } from "svelte/transition";

	export let state;

	export let loop = true;

	const srcs = [
		"null", //0
		"/sound/1Draw.mp3", //1
		"/sound/0920 Who5.mp3", //2
		"/sound/1Base.mp3", //3
		"/sound/0906 2Base.mp3", //4
		"/sound/0906 2Draw.mp3", //5
	];
</script>

{#if srcs[state] != "null"}
	<audio out:fade>
		<audio autoplay {loop}>
			<source src={srcs[state]} type="audio/mpeg" />
		</audio>
	</audio>
	<!-- 	<h1>loop: {loop}, source: {srcs[state]}</h1> -->
{/if}

<style>
	audio {
		display: none;
	}
</style>

<script>
	export let state;

	const srcs = [
		"/sound/1Base.mp3",
		"/sound/1Draw.mp3",
		"/sound/0906 Who5.mp3",
		"/sound/0906 2Base.mp3",
		"/sound/0906 2Draw.mp3",
	];
</script>

<audio>
	<audio autoplay loop>
		<source src={srcs[state]} type="audio/mpeg" />
	</audio>
</audio>

<style>
	audio {
		display: none;
	}
</style>

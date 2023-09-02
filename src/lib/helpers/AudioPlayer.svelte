<script>
	export let src;
</script>

<audio>
	<audio autoplay loop>
		<source {src} type="audio/mpeg" />
	</audio>
</audio>

<style>
	audio {
		display: none;
	}
</style>

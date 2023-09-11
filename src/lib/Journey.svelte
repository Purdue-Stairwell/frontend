<script>
	import Header from "./Header.svelte";
	import LoopingAnim from "./vbox/LoopingAnim.svelte";
	import OneShotAnim from "./vbox/OneShotAnim.svelte";
	import Squiggle from "./vbox/Squiggle.svelte";
	import Narration from "./ibox/Narration.svelte";
	import DefaultButton from "./abox/DefaultButton.svelte";
	import WhoFive from "./vbox/WhoFive.svelte";
	import { fade } from "svelte/transition";
	import { createEventDispatcher, onMount } from "svelte";

	import SocketIO from "socket.io-client";
	import AudioPlayer from "./helpers/AudioPlayer.svelte";

	const dispatch = createEventDispatcher();

	//live URL
	const socket = SocketIO("wss://navinate.com", { secure: true });

	//const socket = SocketIO();
	console.log("connected to websocket");

	const script = fetch("/jsonData/script.json").then((res) => res.json());

	let journeyState =
		//0     1     2   3    4    5    6   7   8   9   10  11   12   13    14     15-->
		//anim squig anim anim anim anim who who who who who anim anim squig squig static-->
		[0, 1, 0, 0, 0, 0, 2, 2, 2, 2, 2, 0, 0, 1, 1, 0];

	//             0     1     2      3      4      5      6      7      8      9      10     11     12     13     14     15
	let preAnim = [
		true, //intro
		true, //first squiggle
		false,
		false,
		false,
		false,
		false, //start of who5
		false,
		false,
		false,
		false,
		false,
		false,
		false, //second squiggle
		false,
		false,
	];

	let screenLocation = 0;
	export let reduceMotion;
	let oneShotEnded = false;

	let whoFiveScores = [undefined, undefined, undefined, undefined, undefined];
	let points = [];
	let spriteChoice;
	let colorChoice;

	let screenHeight;

	//PLACEHOLDER
	let attributes = {
		girth: 15,
		color: "#3c3e78",
		opacity: 1,
		speed: 10,
		wiggle: 7,
	};

	function nextPage() {
		oneShotEnded = false;
		if (screenLocation < 15) {
			screenLocation++;
		} else {
			changeStage("end");
		}
	}

	function backPage() {
		oneShotEnded = false;
		if (screenLocation > 0) {
			screenLocation--;
		} else {
			changeStage("start");
		}
	}

	function changeStage(destination) {
		dispatch("changeStage", destination);
	}

	function updateWhoFiveScore(event) {
		whoFiveScores[event.detail.number] = event.detail.answer;
	}

	function updatePoints(event) {
		points = event.detail[0];
		spriteChoice = event.detail[1];
		colorChoice = event.detail[2];
	}

	async function projectSquiggle() {
		try {
			await socket.emit("frontend to backend", points, whoFiveScores, spriteChoice, colorChoice);
			console.log("projecting squiggle");
			nextPage();
		} catch (error) {
			console.log(error);
		}
	}

	//NOT FINISHED
	let squiggle;
	function saveSquiggle() {
		squiggle.saveSketch();
	}

	onMount(() => {
		document.body.addEventListener("click", () => {
			if (screenLocation >= 0 && screenLocation < 15) {
				document.documentElement.style.setProperty("background-color", "#4b48b9");
			}
		});
	});
</script>

<svelte:window bind:innerHeight={screenHeight} />

<main in:fade class={reduceMotion ? "reduceMotion" : "motion"}>
	<!--HEADER-->
	{#if screenHeight > 800}
		<Header {reduceMotion} mode="stairwell" />
	{/if}
	{#key screenLocation}
		<!--INFO BOX-->
		{#await script}
			<p>Loading Script :)</p>
		{:then result}
			<Narration {reduceMotion} scriptPage={result.script[screenLocation]} />
		{/await}
	{/key}

	<!--VISUAL BOX-->
	<!-- 0---1-----2-----3----4----5----6---7---8---9---10--11---12---13----14-----15-->
	<!--anim squig animm anim anim anim who who who who who anim anim squig static static-->

	<!-- IS THERE NOT A PRE ANIM OR IS IT FINISHED OR NO MOTION?-->
	{#if !preAnim[screenLocation] || oneShotEnded || reduceMotion}
		<!-- IS IT AN ANIMATION?-->
		{#if journeyState[screenLocation] === 0}
			<LoopingAnim {screenLocation} {reduceMotion} />
			<!-- IS IT A SQUIGGLE?-->
		{:else if journeyState[screenLocation] === 1}
			{#key screenLocation}
			<Squiggle
				{screenLocation}
				saveMode={screenLocation === 14}
				hex={colorChoice}
				spriteChoice={spriteChoice}
				globalPoints={points}
				bind:this={squiggle}
				on:squiggleDrawn={updatePoints}
				sketchWidth={300}
				sketchHeight={300}
			/>
			{/key}
			<!-- IS IT A WHO FIVE?-->
		{:else if journeyState[screenLocation] === 2}
			{#key screenLocation}
				<WhoFive on:whofive={updateWhoFiveScore} questionNumber={screenLocation - 6} />
			{/key}
		{/if}
	{:else}
		<OneShotAnim {screenLocation} {reduceMotion} bind:oneShotEnded />
	{/if}

	<!--ACTION BOX-->
	<DefaultButton
		disabled={!oneShotEnded && preAnim[screenLocation] && !reduceMotion}
		showProject={screenLocation === 13}
		showSave={screenLocation === 14}
		on:next={nextPage}
		on:project={projectSquiggle}
		on:save={saveSquiggle}
		on:back={backPage}
	/>

	{#key journeyState[screenLocation]}
		{#if screenLocation !== 13}
			<AudioPlayer state={journeyState[screenLocation]} />
		{/if}
	{/key}
	{#if screenLocation >= 11}
		<AudioPlayer state={4} loop={screenLocation >= 14 ? false : true} />
		<AudioPlayer state={5} loop={screenLocation >= 14 ? false : true} />
	{/if}
	<AudioPlayer state={3} loop={screenLocation > 10 ? false : true} />
</main>

<style>
	main {
		margin: 0 auto;
		width: 100%;
		max-width: 800px;
		height: 100vh;
		padding: 0.5rem 0 3rem 0;
		background: rgb(0, 0, 0);
	}

	.reduceMotion {
		background: linear-gradient(180deg, #1f1669 0%, #5654ca 100%);
	}

	.motion {
		background-image: url("/anim/stairwell_background.gif");
		background-position: center;
		background-size: cover;
	}
</style>

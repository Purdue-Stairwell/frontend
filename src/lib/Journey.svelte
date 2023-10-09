<script>
	import LoopingAnim from "./vbox/LoopingAnim.svelte";
	import OneShotAnim from "./vbox/OneShotAnim.svelte";
	import Squiggle from "./vbox/Squiggle.svelte";
	import StyleNarration from "./ibox/StyleNarration.svelte";
	import DefaultButton from "./abox/DefaultButton.svelte";
	import WhoFive from "./vbox/WhoFive.svelte";
	import { fade } from "svelte/transition";
	import { createEventDispatcher } from "svelte";

	import SocketIO from "socket.io-client";
	import AudioPlayer from "./helpers/AudioPlayer.svelte";

	const dispatch = createEventDispatcher();

	//live URL
	const socket = SocketIO("wss://navinate.com", { secure: true });
	//local backend testing URL
	//const socket = SocketIO("http://localhost:3000", { secure: false });

	//const socket = SocketIO();
	console.log("connected to websocket");

	//old script code
	/* const script = fetch("/jsonData/script.json").then((res) => res.json()); */

	const styleScript = fetch("/jsonData/styleScript.json").then((res) => res.json());

	let journeyState =
		//0     1     2   3    4    5    6   7   8   9   10  11   12   13    14    15    16
		//anim squig anim anim anim anim who who who who who anim anim squig squig squig squig
		[0, 1, 0, 0, 0, 0, 2, 2, 2, 2, 2, 0, 0, 1, 1, 1, 1];
	const saveScreen = 16;
	const projectScreen = 15;

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
		false, //customize squiggle
		false,
		true, //project screen
		false, //save screen
	];

	let screenLocation = 0;
	export let reduceMotion;
	let oneShotEnded = false;

	let whoFiveScores = [0, 0, 0, 0, 0];
	let points = [];
	let spriteChoice = '/anim/empty.gif';
	let baseChoice = '/anim/empty.gif';
	let colorChoice = '#4d26db';
	export let isOver18 = false;

	let screenHeight;

	function nextPage() {
		oneShotEnded = false;
		if (screenLocation < 16) {
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
		baseChoice = event.detail[3];
	}

	async function projectSquiggle() {
		try {
			await socket.emit("frontend to backend", points, whoFiveScores, spriteChoice, colorChoice, isOver18, baseChoice);
			console.log("projecting squiggle");
			console.log("over 18?", isOver18);
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
</script>

<svelte:window bind:innerHeight={screenHeight} />

<main in:fade class={reduceMotion ? "reduceMotion" : "motion"}>
	{#key screenLocation}
		<!--INFO BOX-->
		{#await styleScript}
			<p>Loading Script :)</p>
		{:then result}
			<StyleNarration {reduceMotion} scriptPage={result.script[screenLocation]} />
		{/await}
	{/key}

	<!--VISUAL BOX-->
	<!-- 0---1-----2-----3----4----5----6---7---8---9---10--11---12---13----14-----15---16-->
	<!--anim squig animm anim anim anim who who who who who anim anim squig squiq squiq squig-->

	<!-- IS THERE NOT A PRE ANIM OR IS IT FINISHED OR NO MOTION?-->
	{#if (!preAnim[screenLocation] || oneShotEnded || reduceMotion ) || (screenLocation == 1 && points.length > 0)}
		<!-- IS IT AN ANIMATION?-->
		{#if journeyState[screenLocation] === 0}
			<LoopingAnim {screenLocation} {reduceMotion} />
			<!-- IS IT A SQUIGGLE?-->
		{:else if journeyState[screenLocation] === 1}
			{#key screenLocation}
			<Squiggle
				{screenLocation}
				saveMode={screenLocation === saveScreen}
				hex={colorChoice}
				spriteChoice={spriteChoice}
				baseChoice={baseChoice}
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
		disabled={!oneShotEnded && preAnim[screenLocation] && !reduceMotion && (screenLocation == 1 || screenLocation == 15) && points.length == 0}
		showProject={screenLocation === projectScreen}
		showSave={screenLocation === saveScreen}
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

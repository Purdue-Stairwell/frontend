<script>
    import Header from "./Header.svelte";
    import StaticImage from ".//vbox/StaticImage.svelte";
    import Animation from "./vbox/Animation.svelte";
    import Squiggle from "./vbox/Squiggle.svelte";
    import Narration from "./ibox/Narration.svelte";
    import CreditsButton from "./abox/CreditsButton.svelte";
    import NextButton from "./abox/NextButton.svelte";
    import WhoFive from "./vbox/WhoFive.svelte";
    import ProjectButton from "./abox/ProjectButton.svelte";
    import Resources from "./Resources.svelte";
    import { fade } from "svelte/transition";

    import SocketIO from "socket.io-client";

    //live URL
    const socket = SocketIO("wss://navinate.com", {secure: true});
    
    //const socket = SocketIO();
    console.log("connected to websocket");

    const script = fetch("script.json").then((res) => res.json());

    let journeyState = 
    //0     1     2     3     4    5    6    7   8   9   10  11  12   13    14   15-->
    //anim squig animm anim anim anim who who who who who anim anim squig static static-->
    [0,    1,     0,   0,   0,   0,   2,  2,  2,  2,  2,  0,   0,   1,    0,     0]

    let screenLocation = 0;
    export let reduceMotion;

    let credits = false;

    let whoFiveScores = [5, 5, 5, 5, 5];
    let points = [];

    function nextPage() {
        if (screenLocation < 18) screenLocation++;
    }

    function updateWhoFiveScore(event) {
        whoFiveScores[event.detail.number] = event.detail.answer;
    }

    function updatePoints(event) {
        points = event.detail;
    }

    async function projectSquiggle() {
        try {
            await socket.emit("frontend to backend", points, whoFiveScores)
            console.log('projecting squiggle');
            nextPage();
        } catch (error) {
            console.log(error);
        }
        
    }

    //sends data
    function sendData() {}
</script>

<main in:fade>
    <!--HEADER-->
    <Header mode="stairwell" />
    {#if !credits}
        {#key screenLocation}
            <!--INFO BOX-->
            {#await script}
                <p>Loading Script :)</p>
            {:then result}
                <Narration scriptPage={result.script[screenLocation]} />
            {/await}
        {/key}
        <!--VISUAL BOX-->
        <!-- 0     1     2     3     4    5    6    7   8   9   10  11  12   13    14   15-->
        <!--anim squig animm anim anim anim who who who who who anim anim squig static static-->
        {#if journeyState[screenLocation] == 0}
            <StaticImage {screenLocation} />
        {:else if journeyState[screenLocation] == 2}
            <WhoFive
                on:whofive={updateWhoFiveScore}
                questionNumber={screenLocation - 7}
                value={whoFiveScores[screenLocation - 7]}
            />
        {:else if screenLocation == 2 || screenLocation == 14}
            <Squiggle globalPoints={points}
                on:squiggleDrawn={updatePoints}
                sketchWidth={300}
                sketchHeight={300}
            />
        {:else}
            <Animation />
        {/if}
        <!--ACTION BOX-->
        {#if screenLocation == 18}
            <CreditsButton on:credits={() => (credits = true)} />
        {:else if screenLocation == 14}
            <ProjectButton on:message={nextPage} on:project={projectSquiggle} />
        {:else}
            <NextButton on:message={nextPage} />
        {/if}
    {:else}
        <Resources />
    {/if}
</main>

<style>
    main {
        background-color: #ffffff;
        margin: 0 auto;
        width: 100%;
        max-width: 500px;
        height: 100%;
    }
</style>

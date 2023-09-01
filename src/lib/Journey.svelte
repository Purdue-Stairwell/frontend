<script>
    import Header from "./Header.svelte";
    import Animation from "./vbox/Animation.svelte";
    import Squiggle from "./vbox/Squiggle.svelte";
    import Narration from "./ibox/Narration.svelte";
    import NextButton from "./abox/NextButton.svelte";
    import WhoFive from "./vbox/WhoFive.svelte";
    import ProjectButton from "./abox/ProjectButton.svelte";
    import { fade } from "svelte/transition";
    import { createEventDispatcher } from "svelte";

    import SocketIO from "socket.io-client";

    const dispatch = createEventDispatcher();

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

    let whoFiveScores = [0,0,0,0,0];
    let points = [];

    //PLACEHOLDER
    let attributes = {
        girth: 15,
        color: "#3c3e78",
        opacity: 1,
        speed: 10,
        wiggle: 7
    }

    function nextPage() {
        if (screenLocation < 15) screenLocation++;
        else changeStage();
    }

    function changeStage() {
        dispatch("changeStage", "end");
    }

    function updateWhoFiveScore(event) {
        whoFiveScores[event.detail.number] = event.detail.answer;
        console.log("who5",whoFiveScores);
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
        {#if journeyState[screenLocation] === 0}
            <Animation screenLocation={screenLocation} reduceMotion={reduceMotion}/>
        {:else if journeyState[screenLocation] === 1}
            <Squiggle globalPoints={points}
                on:squiggleDrawn={updatePoints}
                sketchWidth={330}
                sketchHeight={330}
            />
        {:else if journeyState[screenLocation] === 2}
            {#key screenLocation}
            <WhoFive
                on:whofive={updateWhoFiveScore}
                questionNumber={screenLocation - 6}
            />
            {/key}
        {/if}
        <!--ACTION BOX-->
        {#if journeyState[screenLocation] === 1 && screenLocation > 10}
            <ProjectButton on:message={nextPage} on:project={projectSquiggle} />
        {:else}
            <NextButton on:message={nextPage} />
        {/if}
</main>

<style>
    main {
        margin: 0 auto;
        width: 100%;
        max-width: 500px;
        height: 100%;
        background: rgb(0,0,0);
        background: linear-gradient(180deg, rgba(0,0,0,1) 0%, rgba(0,0,0,1) 33%, rgba(60,62,121,1) 100%);
    }
</style>

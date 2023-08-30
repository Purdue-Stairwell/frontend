<script>
    import Header from "./lib/Header.svelte";
    import StaticImage from "./lib/vbox/StaticImage.svelte";
    import Animation from "./lib/vbox/Animation.svelte";
    import Squiggle from "./lib/vbox/Squiggle.svelte";
    import Narration from "./lib/ibox/Narration.svelte";
    import CreditsButton from "./lib/abox/CreditsButton.svelte";
    import NextButton from "./lib/abox/NextButton.svelte";
    import WhoFive from "./lib/vbox/WhoFive.svelte";
    import ProjectButton from "./lib/abox/ProjectButton.svelte";
    import Resources from "./lib/Resources.svelte";

    import SocketIO from "socket.io-client";

    //live URL
    const socket = SocketIO("wss://navinate.com", {secure: true});
    
    //const socket = SocketIO();
    console.log("connected to websocket");

    const script = fetch("script.json").then((res) => res.json());

    let screenLocation = 0;

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

<main>
    <!--HEADER-->
    <Header />
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
        <!-- 0       1     2     3     4    5    6    7   8   9   10  11  12   13    14   15     16-->
        <!--static static squig animm anim anim anim who who who who who anim anim squig static static-->
        {#if screenLocation < 2 || screenLocation > 14}
            <StaticImage {screenLocation} />
        {:else if screenLocation >= 7 && screenLocation <= 11}
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
    <div id="purduep">
        <img src="./justthep.png" alt="Purdue P Logo" />
    </div>
</main>

<style>
    #purduep {
        margin-top: 5px;
    }

    main {
        background-color: #ffffff;
        margin: 0 auto;
        width: 100%;
        max-width: 500px;
        height: 100%;
    }
    img {
        width: 50%;
        margin: 0 auto;
    }

    div {
        width: 100%;
        display: flex;
        justify-content: center;
    }
</style>

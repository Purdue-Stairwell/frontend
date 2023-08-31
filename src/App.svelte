<script>
    import Start from "./lib/Start.svelte";
    import Journey from "./lib/Journey.svelte";
    import End from "./lib/End.svelte";

    let reduceMotion = false;
    function updateMotion(event) {
        reduceMotion = event.detail;
    }

    let stage = "start";
    function changeStage(event) {
        stage = event.detail;
    }
</script>

<main>
    {#if stage == "start"}
        <Start
            reduceMotion={reduceMotion}
            on:changeStage={changeStage}
            on:updateMotion={updateMotion}
        />
    {:else if stage == "journey"}
        <Journey
            reduceMotion={reduceMotion}
            on:changeStage={changeStage}
        />
    {:else if stage == "end"}
        <End />
    {/if}

</main>
<style>

</style>
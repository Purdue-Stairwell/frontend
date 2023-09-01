<script>
    export let mode;
</script>

<main class={mode + "-main"}>
    <div class={mode+"-image"} />
</main>

<style>
    main {
        width: 100%;
        padding: 5px;
        display: flex;
        justify-content: center;
    }
    div {
        
        background-position: center;
        background-size: 90%;
        background-repeat: no-repeat;
        width: 100%;
        max-width: 384px;
        height: 120px;
    }
    .stairwell-image {
        background-image: url("anim/animatedlogo_small.gif");
    }
    .purdue-image {
        background-image: url("/purdue_header.png");
    }
</style>

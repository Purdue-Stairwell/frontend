<script>
    import { fade } from 'svelte/transition';
    export let screenLocation;
    export let reduceMotion;

    let journeyState = 
    //0     1     2    3    4    5    6   7   8   9   10  11   12   13    14   15-->
    //anim squig animm anim anim anim who who who who who anim anim squig static static-->
    [0,    1,     0,   0,   0,   0,   2,  2,  2,  2,  2,  0,   0,   1,    0,     0];

    let srcs = [
        "/anim/barret_idle_w_sound.mp4",
        "SQUIGGLE",
        "/anim/barret_idle_w_sound.mp4",
        "/anim/barret_idle_w_sound.mp4",
        "/anim/barret_idle_w_sound.mp4",
        "/anim/barret_idle_w_sound.mp4",
        "WHO",
        "WHO",
        "WHO",
        "WHO",
        "WHO",
        "/anim/barret_idle_w_sound.mp4",
        "/anim/barret_idle_w_sound.mp4",
        "SQUIGGLE",
        "/kiki.png",
        "/kiki.png", 
    ]
</script>

<main in:fade>
        <!-- svelte-ignore a11y-media-has-caption -->
        {#if screenLocation < 14}
            <video autoplay={!reduceMotion} muted={reduceMotion} loop >
                <source src={srcs[screenLocation]} type="video/mp4">
            </video>
        {:else if screenLocation >= 14}
            <img src={srcs[screenLocation]} alt="Narrative Character reading the text">
        {/if}
</main>

<style>
    main {
        margin: 25px;
        aspect-ratio: 1;
        border-radius: 15px;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 24pt;
    }
    img {
        width: 100%;
        aspect-ratio: 1;
        border-radius: 5px;
    }

    video {
        width: 100%;
        border-radius: 15px;
        aspect-ratio: 1;
        border: #ffffff solid 3px;
        box-shadow: 5px 5px 0px rgba(0,0,0,0.5);
    }
</style>
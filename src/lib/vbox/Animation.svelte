<script>
    import { fade } from 'svelte/transition';
    export let screenLocation;
    export let reduceMotion

    let journeyState = 
    //0     1     2    3    4    5    6   7   8   9   10  11   12   13    14   15-->
    //anim squig animm anim anim anim who who who who who anim anim squig static static-->
    [0,    1,     0,   0,   0,   0,   2,  2,  2,  2,  2,  0,   0,   1,    0,     0]
</script>

<main in:fade>
    {#if screenLocation === 0}
        {#if !reduceMotion}
            <!-- svelte-ignore a11y-media-has-caption -->
            <video autoplay loop >
                <source src="assets/anim.mp4" type="video/mp4">
            </video>
        {:else}
            <video loop muted>
                <source src="assets/anim.mp4" type="video/mp4">
            </video>
        {/if}
    {:else if screenLocation === 2}
    {:else if screenLocation === 3}
    {:else if screenLocation === 4}
    {:else if screenLocation === 5}
    {:else if screenLocation === 11}
    {:else if screenLocation === 12}
    {:else if screenLocation === 14}
    {:else if screenLocation === 15}
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
        border: #ffffff solid 5px;
    }
    img {
        width: 95%;
        aspect-ratio: 1;
        border-radius: 15px;
    }
</style>
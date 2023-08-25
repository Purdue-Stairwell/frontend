<script>
import { fade } from 'svelte/transition';

export let screenLocation;
</script>

<main in:fade>
    {#if screenLocation == 1}
        <img src="sculpture.png" alt="concept art of the stairWELL sculpture">
    {:else}
        STATIC IMAGE!
    {/if}

</main>

<style>
    main {
        margin: 25px;
        aspect-ratio: 1;
        border-radius: 15px;
        background-color: rgb(159, 138, 199);
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 24pt;
        box-shadow: 3px 3px 2px #000015;
    }
    img {
        width: 95%;
        aspect-ratio: 1;
        border-radius: 15px;
    }
</style>
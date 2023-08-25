<script>
import { fade } from 'svelte/transition';

export let screenLocation;
</script>

<main in:fade>
    {#if screenLocation == 0}
        <img src="stairs.png" alt="Abstract representation of the StairWELL sculpture">
    {:else if screenLocation == 1}
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
        border: #2c2e35 solid 5px;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 24pt;
    }
    img {
        width: 95%;
        aspect-ratio: 1;
        border-radius: 10px;
    }
</style>
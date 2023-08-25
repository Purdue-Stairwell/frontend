<script>
import { fade, fly } from 'svelte/transition';
</script>

<main transition:fade>
    SQUIGGLE
</main>

<style>
    main {
        width: 100%;
        aspect-ratio: 1;
        background-color: rgb(199, 138, 138);
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 24pt;
    }
</style>
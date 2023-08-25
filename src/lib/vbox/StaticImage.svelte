<script>
import { fade, fly } from 'svelte/transition';
</script>

<main transition:fade>
STATIC IMAGE
</main>

<style>
    main {
        width: 100%;
        aspect-ratio: 1;
        background-color: rgb(159, 138, 199);
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 24pt;
    }
</style>
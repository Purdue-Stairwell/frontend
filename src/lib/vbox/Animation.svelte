<script>
import { fade, fly } from 'svelte/transition';
</script>

<main transition:fade>
    ANIMATION
</main>

<style>
    main {
        width: 100%;
        aspect-ratio: 1;
        background-color: rgb(138, 199, 161);
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 24pt;
    }
</style>
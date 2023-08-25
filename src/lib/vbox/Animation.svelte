<script>
import { fade } from 'svelte/transition';

</script>

<main in:fade>
    <img src="kiki.png" alt="drawing of kiki the emotional guide">
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
        border: #2c2e35 solid 5px;
    }
    img {
        width: 95%;
        aspect-ratio: 1;
        border-radius: 15px;
    }
</style>
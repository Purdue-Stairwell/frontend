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
        background-color: rgb(138, 199, 148);
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 24pt;
        box-shadow: 3px 3px 2px #000015
    }
    img {
        width: 95%;
        aspect-ratio: 1;
        border-radius: 15px;
    }
</style>
<script>
    import { fade, fly } from 'svelte/transition';
    //import Typewriter from 'typewriter-effect/dist/core';
    const script = fetch('script.json').then(res => res.json());

    export let scriptPage;
    
</script>

<main>
    <p transition:fade>{scriptPage}</p>
</main>

<style>
    main {
        width: fit-content;
        height: 200px;
        background-color: #90D7FF;
        padding: 10px;
        border-radius: 15px;
        margin: 15px;

    }
</style>
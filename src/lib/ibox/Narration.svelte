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
        width: 100%;
        height: 200px;
        background-color: aquamarine;
        padding: 10px;
        font-size: 100%;
    }
</style>
<script>
    import { fade, fly } from 'svelte/transition';
    //import Typewriter from 'typewriter-effect/dist/core';

    export let scriptPage;
    
</script>

<main>
    <p transition:fade>{scriptPage}</p>
</main>

<style>
    main {
        
        display: flex;
        justify-content: space-around;
        background-color: #90D7FF;
        align-items: center;
        border-radius: 15px;
        margin: 15px 25px;
        box-shadow: 3px 3px 2px #000015;
    }
    p{
        line-height: 1.5;
        font-size: 14pt;
        height: 150px;
        background-color: #90D7FF;
        padding: 10px;
        border-radius: 15px;
        font-weight: 700;
        flex: 1;
    }
</style>
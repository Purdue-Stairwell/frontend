<script>
    //import Typewriter from 'typewriter-effect/dist/core';
    const script = fetch('script.json').then(res => res.json());

    export let scriptPage;
    
</script>

<main>
    {#await script}
        <p>Loading Text :)</p>
    {:then result}
        <p>{result.script[scriptPage]}</p>
    {/await}
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
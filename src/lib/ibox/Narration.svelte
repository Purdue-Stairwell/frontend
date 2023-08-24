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
        width: 100%;
        height: 200px;
        background-color: aquamarine;
        padding: 10px;

    }
</style>
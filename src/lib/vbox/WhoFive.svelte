<script>
    
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    let value = 5;

    function updateAnswer(event) {
        dispatch('whofive', {number: questionNumber, answer: event.target.value});
    }

    export let questionNumber;
    

</script>
<main>
    <p>0</p>
    <input type="range" min="0" max="10" bind:value on:change={updateAnswer}>
    <p>10</p>
</main>
<style>
    main {
        width: 100%;
        aspect-ratio: 1;
        padding: 10px;
        display: flex;
        justify-content: center;
        font-size: 24pt;
    }
    input {
        margin: 0 10px;
        width: 70vw;
    }
</style>
<script>
    import { fade } from 'svelte/transition';
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let value = 5;

    function updateAnswer(event) {
        dispatch('whofive', {number: questionNumber, answer: event.target.value});
    }

    export let questionNumber;
    

</script>
<main in:fade>
    <p>0</p>
    <input type="range" min="0" max="10" bind:value on:change={updateAnswer}>
    <p>10</p>
</main>
<style>
    main {
        margin: 25px;
        aspect-ratio: 1;
        border-radius: 15px;
        background-color: rgb(159, 138, 199);
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 24pt;
        box-shadow: 3px 3px 2px #000015;
        padding: 0 10px;

    }
    input {
        margin: 0 10px;
        width: 70vw;
    }
</style>
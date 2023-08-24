<script>
    
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    let value = 10;

    function updateAnswer(event) {
        dispatch('whofive', {number: questionNumber, answer: event.target.value});
    }
    
    export let questionNumber;
    

</script>
<main>
    <input type="range" min="0" max="10" bind:value on:change={updateAnswer}>
</main>
<style>

</style>
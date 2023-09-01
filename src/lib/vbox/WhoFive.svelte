<script>
    import { fade } from 'svelte/transition';
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    const options = ["All of the time", "Most of the time", "More than half the time", "Less than half the time", "Some of the time", "At no time"]

    let answer;

    function updateAnswer(event) {
        dispatch('whofive', {number: questionNumber, answer: event.target.value});
    }

    export let questionNumber;
    

</script>
<main in:fade>
    <ul>
    {#each [5, 4, 3, 2, 1, 0] as number}
        <li>
            <label>
                <input
                    type="radio"
                    name="who5"
                    value={number}
                    bind:group={answer}
                />
                {options[number]}
            </label>
        </li>
    {/each}
</ul>
 
</main>
<style>
    main {
        margin: 25px;
        aspect-ratio: 1;
        border-radius: 15px;
        background-color: #90d7ff;
        font-size: 24pt;
        box-shadow: 3px 3px 2px #000015;
        padding: 0 10px;
    }
</style>
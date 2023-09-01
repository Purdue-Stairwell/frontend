<script>

import { createEventDispatcher } from 'svelte';

const dispatch = createEventDispatcher();

export let text = "Next";

function goNext() {
    dispatch('next');
}

</script>
<main>
    <button on:click={goNext}>
        {text}
    </button>
</main>
<style>

    main {
        width: 100%;
        display: flex;
        justify-content: flex-end;
        padding-right: 2rem;
    }
    button {
            border: #ffffff 3px solid;
            border-radius: 15px;
            box-shadow: 5px 5px 0px rgba(0,0,0,0.5);
            font-size: 32pt;
            background-color: #3ae7c7;
            padding: 5px 15px;
            color: #ffffff;
            font-weight: bold;
            font-family: 'Dosis', sans-serif;
            transition-duration: 0.1s;
    }

    button:hover, button:active, button:focus {
            background-color: #a8ffef;
            color: #000000;
    }
</style>
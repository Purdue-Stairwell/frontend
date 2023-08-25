<script>

import { createEventDispatcher } from 'svelte';

const dispatch = createEventDispatcher();

function goNext() {
    dispatch('message');
}

</script>
<main>
<button on:click={goNext}>
    Next
</button>
</main>
<style>
    main {
        display: flex;
        justify-content: flex-end
    }
    button {
        border: none;
        border-radius: 15px;
        box-shadow: 3px 3px 2px #000015;
        font-size: 32pt;
        background-color: #061826;
        padding: 5px 15px;
        margin: 0px 25px;
        color: #ffffff;
        font-weight: bold;
    }
</style>
<script>
import { createEventDispatcher } from 'svelte';

const dispatch = createEventDispatcher();

let isOver18;
let isPurdueStudent;
function checkForIRB(event) {
    let IRBStatus = false;
    if (isOver18 !== null && isOver18 === "true") {
        if (isPurdueStudent !== null && isPurdueStudent === "w") {
            IRBStatus = true;
        }
    }
    
    dispatch("updateIRB", IRBStatus);
}

</script>
<p>Are you a current Purdue Student?</p>
<div class="option-container">
    <input id="wl" type="radio" bind:group={isPurdueStudent} value="w" on:change={checkForIRB} />
    <label for="wl">West Lafeyette</label>
    <input id="pg" type="radio" bind:group={isPurdueStudent} value="p" on:change={checkForIRB} />
    <label for="pg">Purdue Global</label>
    <input id="na" type="radio" bind:group={isPurdueStudent} value="n" on:change={checkForIRB} />
    <label for="na">Neither</label>
</div>
<p>Are you 18 or older?</p>
<div class="option-container">
    <input id="yes" type="radio" bind:group={isOver18} value="true" on:change={checkForIRB} />
    <label for="yes">Yes</label>
    <input id="no" type="radio" bind:group={isOver18} value="false" on:change={checkForIRB} />
    <label for="no">No</label>
</div>

<style>
    .option-container {
        display: flex;
        flex-flow: row nowrap;
        justify-content: flex-start;
        align-items: center;
        gap: 0.5rem;
        margin-bottom: 0.25rem;
    }
    input[type="radio"] {
        display: none;
    }
    label {
        display: inline-block;
        cursor: pointer;
        border: 2px solid rgb(142, 111, 62);
        padding: 0.25rem 0.5rem;
    }

    input[type="radio"]:checked+label {
        background-color: rgb(142, 111, 62);
        color: black;
    }

</style>
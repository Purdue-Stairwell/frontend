<script>
    import Header from './Header.svelte';
    import { fade } from 'svelte/transition';
    import Resources from './Resources.svelte';
    import PurdueButton from './helpers/PurdueButton.svelte';
    import Sponsors from './Sponsors.svelte';
    import Credits from './Credits.svelte';

    export let reduceMotion;
    let endState = 'Back';

    function changeEndState (event) {
        console.log('end state changed: ', event.detail);
        endState = event.detail;
    }
    
</script>

<main in:fade>
    <!--HEADER-->
    <Header mode="purdue" />
    {#if endState === 'Back'}
        <p>Purdue has an amazing collection of people and services to help you thrive. Check out this portal…</p>
    <PurdueButton on:click={changeEndState} text="Resources" />
    <PurdueButton on:click={changeEndState} text="Sponsors" />
    <PurdueButton on:click={changeEndState} text="Credits" />
    {:else if endState === 'Resources'}
        <PurdueButton on:click={changeEndState} text="Back" />
        <Resources on:changeState={changeEndState}/>
    {:else if endState === 'Sponsors'}
        <PurdueButton on:click={changeEndState} text="Back" />
        <Sponsors on:changeState={changeEndState}/>
    {:else if endState === 'Credits'}
        <PurdueButton on:click={changeEndState} text="Back" />
        <Credits on:changeState={changeEndState}/>
    {/if}
    
</main>

<style>
    * {
        font-family: 'United-Sans', sans-serif;
    }
    main {
        background-color: #ffffff;
        margin: 0 auto;
        width: 100%;
        max-width: 500px;
        height: 100vh;
        padding: 0 1rem;
        display: flex;
        flex-flow: column nowrap;
        gap: 1rem;
    }

    p {
        text-align: center;
        font-size: 2rem;
    }
    img {
        width: 100%;
        margin: 0 auto;
    }

    
</style>
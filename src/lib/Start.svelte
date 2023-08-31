<script>
    import { createEventDispatcher } from 'svelte';
    import Header from './Header.svelte';
    import Toggle from './helpers/Toggle.svelte';
    import { fade } from 'svelte/transition';

    const dispatch = createEventDispatcher();

    export let reduceMotion;

    function updateMotion(event) {
        console.log('motion updated: ', event.detail)
        dispatch('updateMotion', event.detail);
    }

    function changeStage() {
        dispatch('changeStage', 'journey');
    }
</script>

<main in:fade>
    <!--HEADER-->
    <Header mode="purdue" />
    <p>Welcome to StairWELL, an interactive sculpture and gathering place.</p>
    <img
            src="stairs.png"
            alt="Abstract representation of the StairWELL sculpture"
    />

    <button on:click={changeStage}>
        Enter!
    </button>
    <div>
        <Toggle on:stateChange={updateMotion}/>
        <p>Toggle Reduce Motion</p>
    </div>
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
        height: 100%;
        padding: 0 1rem;
        display: flex;
        flex-flow: column nowrap;
        gap: 2rem;
    }

    p {
        text-align: center;
        font-size: 2rem;
    }
    img {
        width: 100%;
        margin: 0 auto;
    }

    button {
        padding: 10px 2px;
        background: none;
        border: 2px solid rgb(142, 111, 62);
        font-size: 18px;
        font-weight: 400;
    }

    button:hover, button:active {
        background-color: rgb(142, 111, 62);
        color: white;
    }

    div {
        position: absolute;
        width: 95%;
        bottom: 0px;
        display: flex;
        justify-content: space-around;
        gap: 0.5rem;
        padding: 1rem 1rem;
    }

    div p {
        font-size: 1.5rem;
        padding-top: 0.2rem;
    }
</style>
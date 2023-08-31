<script>
    import { fade } from "svelte/transition";
    import Modal from "./helpers/Modal.svelte";
    
    const resourceInfo = fetch("/sponsorInfo.json").then((res) => res.json());

    let showModal = false;
    let index = 0;
</script>
<main in:fade>
    {#await resourceInfo}
        <h1>Loading Resource Info...</h1>
    {:then result}
        {#each result.sponsorInfo as sponsor, i}
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <!-- svelte-ignore a11y-no-static-element-interactions -->
            {#if !showModal}
                <!--  -->
                <button class="sponsor-button" on:click={()=>{
                    showModal = true;
                    index = i;
                }}>
                <img src={sponsor.image} alt="Logo for {sponsor.name}" />
            </button>
            {:else}
            <button class="sponsor-button">
            <img src={sponsor.image} alt="Logo for {sponsor.name}" />
            </button>
            {/if}
        {/each}
        <Modal bind:showModal>
            <div class="modal-container">
                <img src={result.sponsorInfo[index].image} alt="Logo for {result.sponsorInfo[index].name}" />
                <p>{result.sponsorInfo[index].info}</p>
                <a href={result.sponsorInfo[index].link}>Link to Resource</a>
            </div>
        </Modal>
    {/await}
</main>
<style>
    /*full width, full height, grid 1 column, 6 rows */
    main {
        display: flex;
        flex-flow: row wrap;
        gap: 0.2rem;
    }

    img {
        object-fit:contain;
        max-height: 150px;
        max-width: 100%;
    }
    .modal-container {
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: center;
        height: 60vh;
    }

    .modal-container > * {
        margin: 15px 0px;
    }

    .sponsor-button {
        background-color: white;
        margin: 0;
        cursor: pointer;
        padding: 1rem;
        width: 100%;
        height: auto;
        border: 2px solid rgb(142, 111, 62);

    }
</style>
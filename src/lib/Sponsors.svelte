<script>
    import { fade } from "svelte/transition";
    import Modal from "./helpers/Modal.svelte";

    let showModal = false;
    let index;

    let names = ["", "", "", "", "", "", ""];
    let links = [];
    let srcs = [
        "/Convos_Black.png",
        "/ROBERTS.png",
        "/CounPsy_H-Full-RGB_1.png",
        "/LSIS_H-Full-RGB_1.png",
        "/Polytech_H-Full-RGB.png",
        "/STL_H-Full-RGB.png",
        "/Theatre_H-Full-RGB.png",
        ];
</script>
<main in:fade>
    {#each srcs as imageSRC, i}
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        {#if !showModal}
            <!--  -->
            <button class="sponsor-button" on:click={()=>{
                showModal = true;
                index = i;
            }}>
            <img src={imageSRC} alt="Logo for {names[i]}" />
        </button>
        {:else}
        <button class="sponsor-button">
        <img src={imageSRC} alt="Logo for {names[i]}" />
        </button>
        {/if}
    {/each}
    <Modal bind:showModal>
        <div class="modal-container">
            <img src={srcs[index]} alt="Logo for {names[index]}" />
            <p>INFORMATION TO FILL OUT</p>
            <a href={links[index]}>Link to Resource</a>
        </div>
    </Modal>
    
</main>
<style>
    /*full width, full height, grid 1 column, 6 rows */
    main {
        display: flex;
        flex-flow: row wrap;
        gap: 0.2rem;
    }

    img {
        object-fit: contain;
        max-height: 100px;
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
<script>
    import Modal from "./helpers/Modal.svelte";
    let names = ["COUNSELING AND PSYCHOLOGICAL SERVICES(CAPS)", "CENTER FOR ADVOCACY, RESPONSE AND EDUCATION", "[NEED LINK] PURDUE GUIDANCE AND COUNSELING CENTER", "PURDUE UNIVERSITY STUDENT HEALTH SERVICE (PUSH)",
    "RECWELLWELLNESS", "STUDENT OF CONCERN REPORT", "OFFICE OF THE DEAN OF STUDENTS", "DISABILITY RESOURCECENTER (DRC)"];
    let links = ["https://www.purdue.edu/caps/", "https://care.purdue.edu/", "", "https://www.purdue.edu/push/", "https://www.purdue.edu/recwell/", "https://www.purdue.edu/advocacy/faculty/incident.html",
    "https://www.purdue.edu/odos/", "https://www.purdue.edu/drc/"];

    let showModal = false;
    let index;
  
</script>
<main>
    <h2>Purdue has an amazing collection of people and services to help you thrive.</h2>
    <h3>Check out this portal...</h3>
    {#each names as name, i}
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        {#if !showModal}
            <div class="modal-button" on:click={() => {
                showModal = true;
                index = i;
                }}>
                <p>{name}</p>
            </div>
            {:else}
                <div class="modal-button">
                    <p>{name}</p>
                </div>
        {/if}

        
    {/each}
    <Modal bind:showModal>
        <div class="modal-container">
            <div class="modal-button">
                <p>{names[index]}</p>
            </div>
            <p>INFORMATION TO FILL OUT</p>
            <a href="{links[index]}">Link to Resource</a>
        </div>
    </Modal>
</main>
<style>

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
  
    main {
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
        height: 75%;
        background-color: #ffffff;
        width: 100%;
        padding-top: 10px;
    }

    h2 {
        margin: 5px 10px 0px 10px;
        text-align: center;
    }

    h3 {
        width: 100%;
        text-align: center;
        font-style: italic;
        padding-bottom: 15px;
        margin-top: 0px;
    }

    .modal-button {
        width: 40%;
        height: 90px;
        background-color: #ddc394;
        color: #2c2e35;
        font-weight: bolder;
        font-size: small;
        padding: 5px;
        border: 5px solid #2c2e35;
        border-radius: 15px;

        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;

        cursor: pointer;
    }

    .modal-container > .modal-button {
        width: 90%;
    }
</style>
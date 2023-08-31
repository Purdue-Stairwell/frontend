<script>
    import { fade } from "svelte/transition";
    import Modal from "./helpers/Modal.svelte";

    let showModal = false;
    let index;
    let teamInfo;

    async function getTeamInfo() {
        teamInfo = await fetch('teamInfo.json')
        .then(res => res.json());
        console.log(teamInfo);
    }
    
    getTeamInfo();
</script>

<main>
    {#await teamInfo}
        <p>Loading Team Data :)</p>
    {:then result}
        {#each result as person}
            <div>
                <img src={person.image} alt={person.name} />
                <h1>{person.name}</h1>
                <p>{person.title}</p>
            </div>
        {/each}
        <Modal bind:showModal>
            <div class="modal-container">
                <img src={teamInfo[index].image} alt={teamInfo[index].name} />
                <h1>{teamInfo[index].name}</h1>
                <p>{teamInfo[index].title}</p>
                <br>
                <p>{teamInfo[index].info}</p>
            </div>
        </Modal>
    {/await}

    
</main>

<style>

</style>
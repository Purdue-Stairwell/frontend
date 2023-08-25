<script>
    let names = ["COUNSELING AND PSYCHOLOGICAL SERVICES(CAPS)", "CENTER FOR ADVOCACY, RESPONSE AND EDUCATION", "[NEED LINK] PURDUE GUIDANCE AND COUNSELING CENTER", "PURDUE UNIVERSITY STUDENT HEALTH SERVICE (PUSH)",
    "RECWELLWELLNESS", "STUDENT OF CONCERN REPORT", "OFFICE OF THE DEAN OF STUDENTS", "DISABILITY RESOURCECENTER (DRC)"];
    let links = ["https://www.purdue.edu/caps/", "https://care.purdue.edu/", "", "https://www.purdue.edu/push/", "https://www.purdue.edu/recwell/", "https://www.purdue.edu/advocacy/faculty/incident.html",
    "https://www.purdue.edu/odos/", "https://www.purdue.edu/drc/"];
</script>
<main>
    <h2>Purdue has an amazing collection of people and services to help you thrive.</h2>
    <h3>Check out this portal...</h3>
    {#each names as name, i}
    <a href={links[i]}>
        <div>
            <p>{name}</p>
        </div>
    </a>
    {/each}
</main>
<style>
    main {
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
        height: 80%;
        background-color: #F5F5F5;
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

    a {
        width: 40%;
        text-decoration: none;
    }

    div {
        width: 100%;
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
    }
</style>
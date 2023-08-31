<script>
    export let mode;
</script>

<main>
    <div class={mode} />
</main>

<style>
    main {
        width: 100%;
        background-color: #ffffff;
        padding: 5px;
        display: flex;
        justify-content: center;
    }
    div {
        
        background-position: center;
        background-size: 90%;
        background-repeat: no-repeat;
        width: 100%;
        max-width: 384px;
        height: 120px;
    }
    .stairwell {
        background-image: url("/stairwell_header.png");
    }
    .purdue {
        background-image: url("/purdue_header.png");
    }
</style>

<script>
    import { fade, fly } from "svelte/transition";
    import { createEventDispatcher, onMount } from "svelte";

    export let scriptPage;

    var i = 0;
    var txt = scriptPage; /* The text */
    var speed = 50; /* The speed/duration of the effect in milliseconds */
    var typed = "";

    function typeWriter() {
        if (i < txt.length) {
            typed += txt.charAt(i);
            i++;
            setTimeout(typeWriter, speed);
        } //....
    }

    onMount(() => {
        typeWriter();
    });
</script>

<main>
    <p>{typed}</p>
</main>

<style>
    main {
        display: flex;
        justify-content: space-around;
        background-color: #90d7ff;
        align-items: center;
        border-radius: 15px;
        margin: 15px 25px;
        box-shadow: 3px 3px 2px #000015;
    }
    p {
        line-height: 1.5;
        font-size: 14pt;
        height: 150px;
        background-color: #90d7ff;
        padding: 10px;
        border-radius: 15px;
        font-weight: 700;
        flex: 1;
    }
</style>

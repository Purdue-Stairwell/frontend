<script>

    import { createEventDispatcher } from 'svelte';
    
    const dispatch = createEventDispatcher();
    
    function displayCredits() {
        //FIX LATER
        alert("Credits");
    }
    
    </script>
    <main>
    <button on:click={displayCredits}>
        Next
    </button>
    </main>
    <style>
        main {
            display: flex;
            justify-content: center
        }
        button {
            border: none;
            border-radius: 25px;
            font-size: 36pt;
            background-color: rgb(32, 126, 27);
            padding: 10px 20px;
            margin: 5px;
            color: white;
        }
    </style>
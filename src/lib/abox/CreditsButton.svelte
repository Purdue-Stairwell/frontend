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
        Credits
    </button>
    </main>
    <style>
        main {
            display: flex;
            justify-content: flex-start
        }
        button {
            border: none;
            border-radius: 15px;
            box-shadow: 3px 3px 2px #000015;
            font-size: 36pt;
            background-color: #FF0035;
            padding: 5px 15px;
            margin: 0px 25px;
            color: #90D7FF;
            font-weight: bold;
        }
    </style>
<script>

    import { createEventDispatcher } from 'svelte';
    
    const dispatch = createEventDispatcher();
    
    function projectSquiggle() {
        dispatch('project');
    }

    function goNext() {
    dispatch('message');
    }
    
    </script>
    <main>
    <button on:click|once={projectSquiggle} id="project">
        Project!
    </button>
    <button on:click={goNext}>
        Next
    </button>
    </main>
    <style>
        main {
            display: flex;
            justify-content: space-between;
            margin: 25px;
        }
        button {
            border: none;
            border-radius: 15px;
            box-shadow: 3px 3px 2px #000015;
            font-size: 32pt;
            background-color: #061826;
            padding: 5px 15px;
            color: #ffffff;
            font-weight: bold;
        }

        #project {
            background-color: #90D7FF;
            color: black;
        }
    </style>
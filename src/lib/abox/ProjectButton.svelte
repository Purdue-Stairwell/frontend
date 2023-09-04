<script>

    import { createEventDispatcher } from 'svelte';
    import NextButton from './NextButton.svelte';
    
    const dispatch = createEventDispatcher();
    
    function projectSquiggle() {
        dispatch('project');
    }

    function nextPage() {
        dispatch('next');
    }
    
    </script>
    <main>
        <NextButton on:next={nextPage} text="Skip" />
        <button on:click|once={projectSquiggle}>
            Project!
        </button>
    
    </main>
    <style>
        main {
            display: flex;
            justify-content: flex-end;
            flex-flow: row no-wrap;
            margin: 25px;
        }
        button {
            border: #ffffff 3px solid;
            border-radius: 15px;
            box-shadow: 5px 5px 0px rgba(0, 0, 0, 0.5);
            font-size: 32pt;
            background-color: #3ae7c7;
            padding: 5px 15px;
            color: #ffffff;
            font-weight: bold;
            font-family: 'Dosis', sans-serif;
            transition-duration: 0.1s;
        }

        button:hover,
        button:active {
            background-color: #a8ffef;
            color: #000000;
        }

        
    </style>
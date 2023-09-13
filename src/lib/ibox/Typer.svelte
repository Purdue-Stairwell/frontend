<script>
    import { createEventDispatcher, onMount } from "svelte";

    const dispatch = createEventDispatcher();

    export let text;
    export let style;
    export let isTyped;

    function typingfinished() {
        dispatch("typingfinished");
    }

    let i = 0;
	let speed = 20; /* The speed/duration of the effect in milliseconds */
	let typed = "";
	let pretyped = text;

	export function typeWriter() {
		if (i < text.length && isTyped) {
			typed += text.charAt(i);
			pretyped = pretyped.slice(1);
			i++;
			setTimeout(typeWriter, speed);
		} else {
			typingfinished();
		}
	}
</script>

{#if isTyped}
    <span style={style}>{typed}<span class="pretyped">{pretyped}</span></span>
{:else}
    <span style={style}>{text}</span> 
{/if}

<style>
    span {
        font-family: "Acumin Pro", sans-serif;
        font-size: 16pt;
		color: white;
		transition-duration: 0.1s;
		text-shadow: 2px 2px 1px rgba(0, 0, 0, 0.5);
	}
	.pretyped {
		color: rgba(255, 255, 255, 0.2);
	}
</style>
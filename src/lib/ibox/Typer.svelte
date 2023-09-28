<script>
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let text;
    export let styleClass;
    export let isTyped;
	export let highlight = false;

    function typingfinished() {
        dispatch("typingfinished");
    }

    let i = 0;
	let speed = 15; /* The speed/duration of the effect in milliseconds */
	let typed = "";
	let pretyped = text;

	export function typeWriter() {
		if (i < text.length && isTyped && !highlight) {
			typed += text.charAt(i);
			pretyped = pretyped.slice(1);
			i++;
			setTimeout(typeWriter, speed);
		} else if (highlight) {
			typed = pretyped;
			pretyped = "";
			typingfinished();
		}else {
			typingfinished();
		}
	}
</script>

{#if isTyped}
	<p class={"default "+styleClass}>{typed}<span class="pretyped">{pretyped}</span></p>
{:else}
    <p class={styleClass}>{text}</p> 
{/if}

<style>
	
	:root {
		--emphasis-color: #17baa4;
	}

	.default {
        font-size: 16pt;
		color: white;
		text-align: center;
	}

	.block {
		display: block;
	}

	.inline {
		display: inline;
	}

	.bold {
		font-weight: bold;
	}

	.larger {
		font-size: 24pt;
	}

	.gap {
		margin-top: 1rem;
	}

	.emphasis {
		color: var(--emphasis-color);
		text-shadow: 0px 0px 10px rgba(255, 255, 255, 1.0);
	}

	.italic {
		font-style: italic;
	}

	.pretyped {
		font-style: inherit;
		font-weight: inherit;
		color: rgba(255, 255, 255, 0.2);
	}
</style>
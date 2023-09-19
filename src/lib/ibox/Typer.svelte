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
	let speed = 20; /* The speed/duration of the effect in milliseconds */
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
    <span class={"default "+styleClass}>{typed}<span class={"pretyped " + styleClass}>{pretyped}</span></span>
{:else}
    <span class={styleClass}>{text}</span> 
{/if}

<style>
	

	.default {
        font-size: 16pt;
		color: white;
		transition-duration: 0.1s;
		text-shadow: 2px 2px 1px rgba(0, 0, 0, 0.5);
	}

	.who-five-emphasis {
		font-weight: bolder;
		font-size: 20pt;
		display: block;
		color: aquamarine;

	}

	.who-five-emphasis.pretyped {
		font-weight: bolder;
		font-size: 20pt;
		display: block;
	}

	.script-emphasis {
		text-align: center;
		display: inline-block;
		font-weight: bolder;
		margin-top: -10px;
		font-size: 30pt;
		color: aquamarine;
		text-shadow: 0px 0px 10px rgba(255, 255, 255, 1.0);
		transition-duration: 1s;
	}

	.script-emphasis.pretyped {
		text-shadow: 0px 0px 0px rgba(255, 255, 255, 0.0);
		color: rgba(0, 0, 0, 0);
		opacity: 0;
		transition-duration: 1s;
	}
	.script-larger {
		font-size: 24pt;
	}

	.script-medium {
		font-size: 20pt;
	}

	.pretyped {
		color: rgba(255, 255, 255, 0.2);
	}
</style>
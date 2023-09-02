<script>
	export let showModal; // boolean

	let dialog; // HTMLDialogElement

	$: if (dialog && showModal) dialog.showModal();
</script>

<!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
<dialog
	bind:this={dialog}
	on:close={() => (showModal = false)}
	on:click|self={() => dialog.close()}
>
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div on:click|stopPropagation>
		<!-- svelte-ignore a11y-autofocus -->
		<button autofocus on:click={() => dialog.close()}>X</button>
		<slot name="header" />
		<slot />
	</div>
</dialog>

<style>
	dialog {
		width: 90dvw;
		border: 2px solid rgb(142, 111, 62);
		margin: auto auto;
	}
	dialog::backdrop {
		background: rgba(0, 0, 0, 0.3);
	}
	dialog > div {
		padding: 1em;
	}
	dialog[open] {
		animation: zoom 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
	}
	@keyframes zoom {
		from {
			transform: scale(0.95);
		}
		to {
			transform: scale(1);
		}
	}
	dialog[open]::backdrop {
		animation: fade 0.2s ease-out;
	}
	@keyframes fade {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}
	button {
		margin-left: calc(100% - 3rem);
		display: block;
		font-size: 24pt;
		border: 2px solid rgb(142, 111, 62);
		background-color: transparent;
		padding: 0rem 0.5rem;
		color: black;
		font-weight: bolder;
	}

	button:hover,
	button:active {
		background-color: rgb(142, 111, 62);
		color: white;
	}
</style>

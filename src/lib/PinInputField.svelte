<script lang="ts">
	import { onMount } from "svelte";

	export let value = "";
	export let field = null;
	export let type: string;
	export let inputmode: string;
	export let placeholder: string;
	export let gotoNextFocus: () => void;
	export let gotoPrevFocus: () => void;
	export let changeValue: (value: string) => void;

	onMount(() => {
		field.type = type;
	});

	const isDeleteKey = (e: KeyboardEvent) =>
		e.key === "Backspace" || e.key === "Delete" || e.key === "MediaLast";
	const isLeftKey = (e: KeyboardEvent) => e.key === "ArrowLeft";
	const isRightKey = (e: KeyboardEvent) => e.key === "ArrowRight";

	const regex = (inputmode: string) => {
		if (inputmode === "text") {
			return new RegExp(/./);
		} else {
			return new RegExp(/^\d+$/);
		}
	};

	const handleFocus = () => {
		field.placeholder = "";
	};
	const handleBlur = () => {
		field.placeholder = "○";
	};
	const handleChange = (e: KeyboardEvent) => {
		if (isDeleteKey(e)) {
			changeValue("");
			gotoPrevFocus();
		} else if (isLeftKey(e)) {
			gotoPrevFocus();
		} else if (isRightKey(e)) {
			gotoNextFocus();
		} else {
			if (e.key.length === 1 && regex(inputmode).test(e.key)) {
				changeValue(e.key);
				gotoNextFocus();
			}
		}
	};
</script>

<input
	bind:this={field}
	bind:value
	class="pin-input-field"
	{placeholder}
	{inputmode}
	maxlength="1"
	on:focus={handleFocus}
	on:blur={handleBlur}
	on:keydown|preventDefault={handleChange}
/>

<style>
	.pin-input-field {
		width: 2.5rem;
		height: 2.5rem;
		text-align: center;
		border: 1px solid rgb(209, 213, 219);
		border-radius: 0.5rem;
	}
</style>

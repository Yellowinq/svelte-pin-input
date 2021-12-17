<script lang="ts">
	import { onMount } from "svelte";
	import PinInputField from "./PinInputField.svelte";

	export let pin = "";
	export let size: number;
	export let isPassword = false;
	export let type: "numeric" | "text" = "numeric";
	export let placeholder = "○";

	let inputs: Array<string> = [];
	let fields: Array<HTMLInputElement> = [];

	onMount(() => {
		inputs = initInputs(size);
		fields = initFields(size);
	});

	const conf: {
		type: string;
		inputmode: string;
		placeholder: string;
	} = {
		type: isPassword ? "password" : type === "numeric" ? "tel" : "text",
		inputmode: type,
		placeholder,
	};

	const initInputs = (size: number) => new Array<string>(size);
	const initFields = (size: number) => new Array<HTMLInputElement>(size);

	const gotoNextFocus = (index: number) => {
		if (index < size - 1) fields[index + 1].focus();
		if (index === size - 1) fields[index].blur();
	};
	const gotoPrevFocus = (index: number) => {
		if (index > 0) fields[index - 1].focus();
	};
	const changeValue = (index: number, value: string) => {
		inputs[index] = value;
		fields[index].value = value;
		handlePin();
	};
	const handlePin = () => {
		pin = inputs.join("");
	};
</script>

<div class="pin-input">
	{#each inputs as _input, i (i)}
		<PinInputField
			bind:field={fields[i]}
			bind:value={inputs[i]}
			{...conf}
			gotoNextFocus={() => gotoNextFocus(i)}
			gotoPrevFocus={() => gotoPrevFocus(i)}
			changeValue={(value) => changeValue(i, value)}
		/>
	{/each}
</div>

<style>
	:global(input[type="tel" i]) {
		padding: 0;
	}
</style>

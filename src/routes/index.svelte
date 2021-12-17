<script lang="ts">
	import PinInput from "$lib/PinInput.svelte";

	let pinCode: string;
</script>

<PinInput bind:pin={pinCode} size={4} />

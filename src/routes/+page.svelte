<script lang="ts">
	import Counter from '$lib/components/Counter.svelte';
	import Pointer from '$lib/components/Pointer.svelte';
	const name = 'bk';
	const myTag = '<button>Sample Button</button>';

	let showCounter = true;

	const handleMessage = (event: CustomEvent<{ text: string }>) => {
		console.log(event.detail.text);
	};
</script>

<svelte:head>
	<title>Home Page</title>
</svelte:head>

<div class="grid place-items-center h-screen">
	<div class="flex flex-col gap-y-3 items-center">
		<h1 class="font-bold">Hello {name.toUpperCase()} {@html myTag}</h1>
		<button
			on:click={() => {
				showCounter = !showCounter;
			}}>{showCounter ? "Hide Counter" : "Show Counter"}</button
		>
		{#if showCounter}
			<Counter on:message={handleMessage} />
		{/if}
		<Pointer x={10} y={20} />
	</div>
</div>

<style lang="postcss">
	h1 {
		@apply text-7xl;
	}
	button {
		@apply border-none py-3 px-5 bg-zinc-700 rounded-lg active:bg-zinc-600 hover:bg-zinc-600;
	}
</style>

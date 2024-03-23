<script lang="ts">
	import { count } from '$lib/store/store';
	import { createEventDispatcher, onDestroy, onMount } from 'svelte';
	import UpdateCounter from './UpdateCounter.svelte';
	import type { Unsubscriber } from 'svelte/store';

	let value: number;
	let values: Array<number> = [];
	$: result = value * 2;

	let unsubscribe: Unsubscriber;

	$: console.log(`Counter Changed to ${value}`);

	$: {
		console.log('Set Of Statements'	);
		if (value > 10) {
			alert('Limit Reached');
			value--;
		} else {
			if (value != undefined) {
				values = [...values, value];
				console.log(values);
			}
		}
	}

	// Create a Function to Return a Simulated Promise
	const getInitialNumber = async () => {
		return new Promise<void>((res, rej) => {
			setTimeout(() => {
				unsubscribe = count.subscribe((val) => {
					value = val;
				});
				res();
			}, 1000);
		});
	};

	// Run the Data Fetching in the initial render
	onMount(() => {
		console.log('First render');
	});
	const init = async () => {
		await getInitialNumber();
	};

	// Create an Event Dispatcher
	const dispatch = createEventDispatcher();

	// Dispatcher Message
	const dispatchMessage = () => {
		dispatch('message', {
			text: 'Success'
		});
	};

	// Run on Unmount
	onDestroy(() => {
		console.log('Component Destroyed');
		if (unsubscribe) {
			unsubscribe();
			console.log('Unsubscribed');
		}
	});
</script>

<div class="py-2 flex flex-col gap-y-3">
	{#await init()}
		<p>Loading...</p>
	{:then res}
		<h1 class="text-7xl text-center font-bold">{value} * 2 = {result}</h1>
		<div class="flex items-center gap-x-3 justify-center">
			<UpdateCounter />
			<UpdateCounter increment={false} />
			<button on:click={dispatchMessage}>Dispatch</button>
		</div>
		{#if 10 - value <= 2 && value < 10}
			<p class="text-red-500">You are almost Close to the Max Value - 10</p>
		{:else if value >= 10}
			<p class="text-red-500 text-center">You have reached the Max Value - 10</p>
		{/if}
	{/await}
</div>

<style lang="postcss">
	button {
		@apply border-none py-3 px-5 bg-zinc-700 rounded-lg active:bg-zinc-600 hover:bg-zinc-600;
	}
</style>

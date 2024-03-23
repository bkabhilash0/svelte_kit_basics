<script lang="ts">
	import { count } from '$lib/store/store';

	export let increment = true;

	const handleChange = () => {
		if (increment) {
			count.update((n) => n + 1);
		} else {
			count.update((n) => n - 1);
		}
	};
</script>

<button on:click={handleChange}>{increment ? 'Increment' : 'Decrement'}</button>

<style lang="postcss">
	button {
		@apply border-none py-3 px-5 bg-zinc-700 rounded-lg active:bg-zinc-600 hover:bg-zinc-600;
	}
</style>

<script lang="ts">
	import { count } from "$lib/store/store";

    export let x:number;
    export let y:number;
    
    let store_value:number;

</script>

<p>The Mouse Position is at ({x},{y})</p>
<p>The store value is {$count}</p>
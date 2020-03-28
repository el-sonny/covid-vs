<script>
	export let countries;	
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function toggleCountry(country){
		dispatch('toggleCountry', {country: country});
	}
</script>

<ul>
	{#each countries as country, i}
		<li >
			<!-- style='background-color:{pallete[i % pallete.length]}'  -->
			<button on:click={toggleCountry(country)}>{country.name}</button>
		</li>
	{/each}
</ul>
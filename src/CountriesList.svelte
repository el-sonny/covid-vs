<script>
	import { createEventDispatcher } from 'svelte';
	export let countries;	
	const dispatch = createEventDispatcher();
	$: filteredList = countries.filter(item => item.name.toLowerCase().indexOf(searchTerm.toLowerCase()) !== -1);	
	let searchTerm = "";

	function diffPercent(country){
		const last = country.data[country.data.length-1]; 
		const penultimate = country.data[country.data.length-2];
		return Math.round(((last - penultimate)/penultimate) * 100);
	}
	function toggleCountry(country){
		console.log(country);
		dispatch('toggleCountry', {country: country});
	}
</script>
<p>
	<input bind:value={searchTerm} />
</p>
<ul>
	{#each filteredList as country}
		<li on:click={toggleCountry(country)}>
			<span class='label'>{country.name}</span>
			<span class='data-point alt'>{diffPercent(country)} %</span>
			<span class='data-point'>{new Intl.NumberFormat().format(country.data[country.data.length-1])}</span>
		</li>
	{/each}
</ul>
<style>
	p{
		text-align: center;
	}
	ul{
		margin:10px 5px;
		padding:0 20px;
	}
	li{
		display:flex;
		list-style: none;
		border:1px solid #000;
		margin-bottom:5px;
		text-align: center;
		height:40px;
		line-height:40px;
		padding:0 0 0 5px;
		width:100%;
		justify-content: flex-end;
		cursor:pointer;

	}
	li .label{
		flex-grow: 1;
		text-align:left;
	}
	li .data-point{
		background-color:#665191;
		color:white;
		width:20%;
		font-weight:bold;
	}
	li .data-point.alt{
		background-color: #669151;
	}
</style>
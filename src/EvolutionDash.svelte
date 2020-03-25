<script>
	import EvolutionGraph from './EvolutionGraph.svelte';
	import { createEventDispatcher } from 'svelte';

	export let countries;

	const dispatch = createEventDispatcher();
	const pallete = ['#EE4266','#2A1E5C','#0A0F0D','#C4CBCA','#3CBBB1','#182825','#016FB9','#22AED1','#6D8EA0','#AFA98D'];	
	const pallete2 = ['#FBDCE3','#D8D6E1','#D2D3D3','#F4F5F5','#DBF2F0','#969D9B','#8BBDDF','#9ADAEA','#BCCBD3','#DAD7CB'];
	
	$: options = {
		scale : 'logarithmic',
		dayZero : true,
		data : 'cases'
	};

	$: labels = countries.length > 0 ? countries[0].labels : [];

	function toggleDayZero(){
		options.dayZero = !options.dayZero;
	}
	function toggleByRate(){
		options.data = options.data === 'rates' ? 'cases' : 'rates';
	}
	function toggleScale(){
		options.scale = options.scale === 'linear' ? 'logarithmic' : 'linear';
	}
	function toggleCountry(country){
		dispatch('toggleCountry', {country: country});
	}
</script>
<h3>Case Evolution by Country</h3>
<nav>		
	<ul>
		{#each countries as country, i}
			<li >
				<button style='background-color:{pallete[i % pallete.length]}' on:click={toggleCountry(country)}>{country.name}</button>
			</li>
		{/each}
	</ul>
	<ul class='controls'>
		<li><button on:click={toggleDayZero}>{options.dayZero ? 'Day Zero' : 'Natural Days'}</button></li>
		<li><button on:click={toggleByRate}>Data: {options.data}</button></li>
		<li><button on:click="{toggleScale}">Scale : {options.scale}</button></li>
	</ul>
</nav>	

<EvolutionGraph countries={countries} labels={labels} options={options}/>

<style>
	nav .controls{
		flex:4;
		justify-content:flex-start;
	}
	nav .controls button{
		background-color:#AAA;
	}
	ul{
		flex: 6;
		display:flex;
		list-style: none;
		margin:20px 0;
  		justify-content: flex-end;
  		padding:0;
	}
	ul li{
		width:33%;
		text-align: center;
		max-width:300px;
		margin:0 0 0 10px;
	}
	ul li button{
		width:100%;
		text-align: center;
		color:white;
	}
</style>
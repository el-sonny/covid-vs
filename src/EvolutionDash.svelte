<script>
	import EvolutionGraph from './EvolutionGraph.svelte';

	export let countries;
	export let chartId = 'myChart';
	
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
</script>

<nav>		
	<ul class='controls'>
		<li><button on:click={toggleDayZero}>{options.dayZero ? 'Day Zero' : 'Natural Days'}</button></li>
		<li><button on:click={toggleByRate}>Data: {options.data}</button></li>
		<li><button on:click="{toggleScale}">Scale : {options.scale}</button></li>
	</ul>
</nav>
<EvolutionGraph countries={countries.filter(c=> c.selectedIndex >= 0)} labels={labels} options={options} chartId={chartId}/>

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
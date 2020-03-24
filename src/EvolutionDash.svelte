<script>
	import EvolutionGraph from './EvolutionGraph.svelte';
	export let countries;

	const pallete = ['#EE4266','#2A1E5C','#0A0F0D','#C4CBCA','#3CBBB1'];	
	const pallete2 = ['#FBDCE3','#D8D6E1','#D2D3D3','#F4F5F5','#DBF2F0'];
	$: options = {
		scale : 'linear',
		dayZero : false,
		data : 'cases'
	};

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
<h3>Case Evolution by Country</h3>
<nav>		
	<ul>
		{#each countries as country, i}
			<li >
				<button style='background-color:{pallete[i % pallete.length]}'>{country.name}</button>
			</li>
		{/each}
	</ul>
	<ul class='controls'>
		<li><button on:click={toggleDayZero}>Day Zero</button></li>
		<li><button on:click={toggleByRate}>Toggle rate</button></li>
		<li><button on:click="{toggleScale}">Toggle scale</button></li>
	</ul>
</nav>	

<EvolutionGraph countries={countries} options={options}/>

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
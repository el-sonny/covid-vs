<script>
	import LineGraph from './LineGraph.svelte';

	export let countries;
	
	const pallete = ['#EE4266','#2A1E5C','#0A0F0D','#C4CBCA','#3CBBB1'];	
	const pallete2 = ['#FBDCE3','#D8D6E1','#D2D3D3','#F4F5F5','#DBF2F0'];

	let byRate = false;
	let logScale = false; 

	$: datasets = countries.map((c,i) => ({
		label: c.name,
        data: !byRate ? c.data : c.data.map(getDerivative),
        borderColor: pallete[i % pallete.length],
        backgroundColor : pallete2[i % pallete2.length]
	}))

	function getDerivative(c,i,arr){
		const der = i > 0 && c > 0 ? Math.round((c - arr[i-1]) / arr[i-1] * 100) : 0;
		return der;
	}

	function toggleByRate(){
		logScale = false;
		byRate = !byRate;
	}
</script>

<h3>Case Evolution by Country</h3>
<nav>	
	<ul class='controls'>
		<li><button on:click={toggleByRate}>Toggle rate</button></li>
		{#if !byRate}
			<li><button on:click="{e => logScale = !logScale}">Toggle scale</button></li>
		{/if}
	</ul>
	<ul>
		{#each countries as country, i}
			<li >
				<button style='background-color:{pallete[i % pallete.length]}'>{country.name}</button>
			</li>
		{/each}
	</ul>
</nav>
<LineGraph labels={countries[0].labels} datasets={datasets} scale={logScale ? 'logarithmic' : 'linear'} />

<style>
	nav{
		margin:10px;
		display:flex;
	}
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
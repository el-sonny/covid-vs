<script>
	import Papa from 'papaparse';
	import EvolutionGraph from './EvolutionGraph.svelte';
	import CountriesList from './CountriesList.svelte';

  	export let dataSource = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv';

  	const pallete = ['#EE4266','#2A1E5C','#0A0F0D','#C4CBCA','#3CBBB1'];	
	const pallete2 = ['#FBDCE3','#D8D6E1','#D2D3D3','#F4F5F5','#DBF2F0'];

  	let selectedCountries = ['Mexico','Bolivia','Korea, South'];	
  	let countries = getCovidData();

	async function getCovidData() {		
		const res = await fetch(dataSource);
		const csv = await res.text();
		const json = Papa.parse(csv,{header: true}).data;
		const formatted = json
			.filter(row => row['Province/State'] === '')
			.map(row => ({
					name : row['Country/Region'],
					lat : row.Lat,
					long : row.Long,
					data : Object.values(row).splice(4),
					labels : Object.keys(row).splice(4),
					filtered : selectedCountries.findIndex(label => row['Country/Region'] === label) >= 0,
				}));
		return formatted;
	}

</script>

{#await countries then countries}
	<div class="with-sidebar">
	  <div>

	    <div><CountriesList countries={countries} /></div> 
	    <div>
	    	<h1>Covid Dashboard</h1>
	    	<EvolutionGraph countries={countries.filter(c => c.filtered)} />
	    </div>

	  </div>
	</div>	
{/await}

<style>

h1{
	text-align:center;
}

.with-sidebar {
  overflow: hidden;
}

.with-sidebar > * {
  display: flex;
  flex-wrap: wrap;
  margin: calc(var(--s1) / 2 * -1);
}

.with-sidebar > * > * {
  margin: calc(var(--s1) / 2);
  flex-grow: 1;

  max-height: 100vh;
  overflow-y:scroll;
  min-width:350px;
}

.with-sidebar > * > :last-child {
  flex-basis: 0;
  flex-grow: 999;
  min-width: calc(50% - var(--s1));
  overflow:hidden;
}
</style>
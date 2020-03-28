<script>
	import EvolutionDash from './EvolutionDash.svelte';
	import CountriesList from './CountriesList.svelte';
	import CountrySelector from './CountrySelector.svelte';
	import * as utils from 'utils.js';

  $: selectedCountries = ['Mexico','Korea, South','Italy','Spain','US','Argentina','Brazil','Chile'];	
  const casesData = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv';
  const deathsData = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv';
  
  const cases = utils.serialize(casesData,20);
  const deaths = utils.serialize(deathsData,1);
    
	function toggleCountry(e){
		const index = selectedCountries.findIndex(label => label === e.detail.country.name);
		selectedCountries = index >= 0 ? selectedCountries.filter(c => c !== e.detail.country.name) : [...selectedCountries,e.detail.country.name];
	}

</script>

<div class="with-sidebar">
  <div>
	{#await cases then countries}
    	<div><CountriesList on:toggleCountry={toggleCountry} countries={utils.serializeSelected(countries,selectedCountries)}  /></div> 
    {/await}
    <div>
    	<h1>Covid Dashboard</h1>
    	{#await cases then countries}
    		<EvolutionDash countries={utils.serializeSelected(countries,selectedCountries)} />
    	{/await}

    	{#await deaths then deaths}
			<EvolutionDash chartId='myChart2' countries={utils.serializeSelected(deaths,selectedCountries)} on:toggleCountry={toggleCountry}/>    	
    	{/await}
    </div>
  </div>
</div>	

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
  overflow-x: hidden;
}
</style>

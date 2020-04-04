<script>
  import EvolutionDash from './EvolutionDash.svelte';
  import EvolutionGraph from './EvolutionGraph.svelte';
	import CountriesList from './CountriesList.svelte';
  import DataControls from './DataControls.svelte';
	import * as utils from 'utils.js';

  $: selectedCountries = ['Brazil','Chile','Italy','Korea, South','Mexico','Spain','US',];	
  $: highlightedCountry = '';
  $: zoomCountry = '';
  $: options = {
    scale : 'logarithmic',
    dayZero : true,
    data : 'cases',
    zoom : 100
  };

  const casesData = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv';
  const deathsData = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv';
  const recoveriesData = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv';
  const mxData = 'https://overflow.ai/coronavirus/api';
  const cases = utils.serialize(casesData);
  const deaths = utils.serialize(deathsData);
  const recoveries = utils.serialize(recoveriesData);
  //const mx = utils.serializeMX(mxData).then((data)=> console.log(data));


	function toggleCountry(e){
		const index = selectedCountries.findIndex(label => label === e.detail.country.name);
    highlightedCountry = highlightedCountry === e.detail.country.name ? '' : highlightedCountry;
		selectedCountries = index >= 0 ? selectedCountries.filter(c => c !== e.detail.country.name) : [...selectedCountries,e.detail.country.name];
	}
  function toggleHighlight(e){
    highlightedCountry = highlightedCountry === e.detail.country.name ? '' : e.detail.country.name;
  }


</script>

<div class="with-sidebar">
  <div><div>
     
	{#await cases then countries}
    	<CountriesList 
        on:toggleCountry={toggleCountry} 
        on:toggleHighlight={toggleHighlight} 
        countries={utils.serializeSelected(countries,selectedCountries,highlightedCountry)}
         />
  {/await}
      <DataControls bind:options={options} />
   </div><div>
    	<h1>Covid Dashboard</h1>
    	<section class='graph'>
        {#await cases then countries}
          <EvolutionGraph 
            countries={utils.serializeSelected(countries,selectedCountries,highlightedCountry,20).filter(c=> c.selectedIndex >= 0)} 
            labels={countries[0].labels} 
            options={options}
            title = "COVID19 Cases"
            yTitle = {options.scale === 'logarithmic' ? "Cases (logarithmic scale)" : "Cases"}          
            xTitle = {options.dayZero ? 'Days From Day Zero (20th Case)' : 'Date'}
            chartId="casesChart" />
      	{/await}
      </section>
      <section class='graph'>
      	{#await deaths then deaths}
    			<EvolutionGraph 
              countries={utils.serializeSelected(deaths,selectedCountries,highlightedCountry,2).filter(c=> c.selectedIndex >= 0)} 
              labels={deaths[0].labels} 
              options={options}
              title = "COVID19 Deaths"
              yTitle = {options.scale === 'logarithmic' ? "Deaths (logarithmic scale)" : "Deaths"}
              xTitle = {options.dayZero ? 'Days From Day Zero (2nd Death)' : 'Date'}
              chartId="deathsChart" />
      	{/await}
      </section>

       <section class='graph'>
        {#await recoveries then recoveries}
          <EvolutionGraph 
              countries={utils.serializeSelected(recoveries,selectedCountries,highlightedCountry,1).filter(c=> c.selectedIndex >= 0)} 
              labels={recoveries[0].labels} 
              options={options}
              title = "COVID19 Recoveries"
              yTitle = {options.scale === 'logarithmic' ? "Recoveries (logarithmic scale)" : "Recoveries"}
              xTitle = {options.dayZero ? 'Days From Day Zero (1st Recovery)' : 'Date'}
              chartId="recoveriesChart" />
        {/await}
      </section>


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
  width:380px;
}

.with-sidebar > * > :last-child {
  flex-basis: 0;
  flex-grow: 999;
  min-width: calc(50% - var(--s1));
  overflow-x: hidden;
}
section.graph{
  padding:0 10px 30px;
}
</style>

<script>
  import EvolutionDash from './EvolutionDash.svelte';
  import EvolutionGraph from './EvolutionGraph.svelte';
	import CountriesList from './CountriesList.svelte';
  import DataControls from './DataControls.svelte';
	import * as utils from 'utils.js';
  import * as config from 'config.js';

  $: selectedDatasetIndex = 0;
  $: selectedDataset = config.datasets[selectedDatasetIndex];
  $: selectedGraph = 0;
  $: selectedCountries = selectedDataset.options.defaultSelected ? selectedDataset.options.defaultSelected : [];
  $: highlightedCountry = '';
  $: options = {
    scale : 'logarithmic',
    dayZero : true,
    data : 'cases',
    zoom : 100
  };

  $: entityList = utils.serialize(selectedDataset.timeSeries[0].url,selectedDataset.options);
  $: currentData = utils.serialize(selectedDataset.timeSeries[selectedGraph].url,selectedDataset.options);

	function toggleCountry(e){
		const index = selectedCountries.findIndex(label => label === e.detail.country.name);
    highlightedCountry = highlightedCountry === e.detail.country.name ? '' : highlightedCountry;
		selectedCountries = index >= 0 ? selectedCountries.filter(c => c !== e.detail.country.name) : [...selectedCountries,e.detail.country.name];
	}
  function toggleHighlight(e){
    highlightedCountry = highlightedCountry === e.detail.country.name ? '' : e.detail.country.name;
  }

  function changeGraph(i){
    selectedGraph = i;
  }


</script>

<div class="with-sidebar">
  <div><div>
      <h3>Datasets</h3>
      <ul class='dataset-list'>
        {#each config.datasets as dataset, i}
        <li>
          {dataset.title}
        </li>
        {/each}
      </ul>
      <h3>Entities</h3>
    	{#await entityList then countries}
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
        <ul class='graph-selector'>
          {#each selectedDataset.timeSeries as graph, i}
            <li><button on:click={() => changeGraph(i)} class={i === selectedGraph ? 'selected' : '' }>{graph.label}</button></li>
          {/each}
        </ul>
        {#await currentData then countries}
          <EvolutionGraph 
            countries={utils.serializeSelected(countries,selectedCountries,highlightedCountry,selectedDataset.timeSeries[selectedGraph].dayZero).filter(c=> c.selectedIndex >= 0)} 
            labels={countries[0].labels} 
            options={options}
            title = {selectedDataset.timeSeries[selectedGraph].title}
            yTitle = {options.scale === 'logarithmic' ? "Cases (logarithmic scale)" : "Cases"}          
            xTitle = {options.dayZero ? 'Days From Day Zero (20th Case)' : 'Date'}
            chartId="casesChart" />
      	{/await}
      </section>
  

    </div>
  </div>
</div>	

<style>

h1{
	text-align:center;
}

h3{
  padding: 0 25px;
}
.dataset-list{
  list-style-type: none;
  padding: 0 25px;
}
.dataset-list li{
  padding:5px 10px;
  margin:2px 0;
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

.graph-selector{
  list-style-type: none;
  display: flex;
  justify-content: flex-end;
}
.graph-selector button{
  background-color: #016FB9;
  margin-left:10px;
  color: white;
  text-transform: capitalize;
  padding:0 20px;
}
.graph-selector button.selected{
  background-color: #EE4266;
}
</style>

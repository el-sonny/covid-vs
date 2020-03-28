<script>
	import LineGraph from './LineGraph.svelte';	

	export let countries;
	export let labels;
	export let chartId = 'myChart'
	export let options = {
		scale : 'logarithmic',
		dayZero : false,
		data : 'cases'
	};

	$: datasets = countries.map((c,i) => {
		const data = options.data === 'cases' ? [...c.data] : [...c.data.map(getDerivative)];
		const trimmedData = options.dayZero ? data.splice(c.dayZero) : data;
		return {
			label: c.name,
	        data: trimmedData,
	        borderColor: c.color,
	        backgroundColor : 'rgba(0, 0, 0, 0)',
    	}
	});

	$: _labels = options.dayZero && countries.length > 0 ? [...Array(labels.length - Math.min(...countries.map(c => c.dayZero)) + 3).keys()] : labels;

	function getDerivative(c,i,arr){
		const der = i > 0 && c > 0 ? Math.round((c - arr[i-1]) / arr[i-1] * 100) : 0;
		return der;
	}
</script>

<LineGraph labels={_labels} datasets={datasets} scale={options.scale} chartId={chartId} />
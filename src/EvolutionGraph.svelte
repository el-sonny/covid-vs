<script>
	import LineGraph from './LineGraph.svelte';

	export let countries;
	export let labels;
	export let options = {
		scale : 'logarithmic',
		dayZero : false,
		data : 'cases'
	};

	const pallete = ['#EE4266','#2A1E5C','#0A0F0D','#C4CBCA','#3CBBB1','#182825','#016FB9','#22AED1','#6D8EA0','#AFA98D'];	
	const pallete2 = ['#FBDCE3','#D8D6E1','#D2D3D3','#F4F5F5','#DBF2F0','#969D9B','#8BBDDF','#9ADAEA','#BCCBD3','#DAD7CB'];

	$: datasets = countries.map((c,i) => {
		const data = options.data === 'cases' ? [...c.data] : [...c.data.map(getDerivative)];
		const trimmedData = options.dayZero ? data.splice(c.dayZero) : data;
		return {
			label: c.name,
	        data: trimmedData,
	        borderColor: pallete[i % pallete.length],
	        backgroundColor : pallete2[i % pallete2.length],
    	}
	});

	$: _labels = options.dayZero && countries.length > 0 ? [...Array(Math.max(...countries.map(c => c.dayZero))).keys()] : labels;

	function getDerivative(c,i,arr){
		const der = i > 0 && c > 0 ? Math.round((c - arr[i-1]) / arr[i-1] * 100) : 0;
		return der;
	}
</script>

<LineGraph labels={_labels} datasets={datasets} scale={options.scale} />
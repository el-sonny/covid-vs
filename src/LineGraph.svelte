<script>
	import Chart from "chart.js";
  	import {onMount, afterUpdate} from 'svelte';
	
	export let labels;
	export let datasets;
	export let scale = 'linear';
	
	let chart;
		
	function renderChart(){
		const ctx = document.getElementById("myChart").getContext("2d");
		chart = new Chart(ctx, {
			type: "line",
			data: {
				labels: labels,
				datasets: datasets
			},
			options: {
				legend : {display: false},
				scales : {
					yAxes: [{
						display: true,
						type: scale,
					}]
				}
			}
		});
	}
	onMount(renderChart);
	afterUpdate(() => {
		chart.destroy();
		renderChart();
	});
</script>
<canvas id="myChart"></canvas>
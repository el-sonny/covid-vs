<script>
	import Chart from "chart.js";
	import 'chartjs-plugin-trendline';
  	import {onMount, afterUpdate} from 'svelte';
	
	export let labels;
	export let datasets;
	export let scale = 'linear';
	export let chartId = 'myChart'
	export let yTitle;
	export let xTitle;
	export let title;
	export let maxY;
	
	let chart;
		
	function renderChart(){
		const ctx = document.getElementById(chartId).getContext("2d");
		let chartjsOptions = {
			type: "line",
			data: {
				labels: labels,
				datasets: datasets
			},
			options: {
				animation: { duration: 0},
				title : {
					display: true,
					fontSize : 20,
					text : title
				},
				legend : {
					display: true,
					labels : {
						boxWidth : 12,
						fontSize : 14,
						usePointStyle : true,
						padding : 15,
					},
				},
				scales : {
					yAxes: [{
						display: true,
						type: scale,
						scaleLabel : {
							display : true,
							labelString : yTitle
						},
						ticks: {
		                    callback: (value) => value >= 1000 ? Math.round(value / 1e3) + ' K' : Math.round(value),
		                },
					}],

					xAxes: [{
						display: true,
						scaleLabel : {
							display : true,
							labelString : xTitle
						},
						ticks: {
		                    maxRotation : 0,
		                    autoSkipPadding : 5,
		                },
					}]
				}
			}
		};
		if(maxY) chartjsOptions.options.scales.yAxes[0].ticks.max = maxY;
		chart = new Chart(ctx, chartjsOptions);
	}
	onMount(renderChart);
	afterUpdate(() => {
		chart.destroy();
		renderChart();
	});
</script>
<canvas id={chartId}></canvas>
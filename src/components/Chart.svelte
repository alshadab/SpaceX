<script>
	import { Chart, Card } from 'flowbite-svelte';
	import { sharedState } from '../shared/store';

	export let landingPadsData = [];


	let currentValue = null;

	let options = {
		series: [],
		colors: ['#16BDCA', '#E74694'],
		chart: {
			height: 320,
			width: '100%',
			type: 'donut'
		},
		stroke: {
			colors: ['transparent']
		},
		plotOptions: {
			pie: {
				donut: {
					labels: {
						show: true,
						total: {
							showAlways: true,
							show: true,
							label: 'Landing Pads',
							fontFamily: 'Inter, sans-serif',
							formatter: function () {
								return `${landingPadsData.length}`;
							}
						}
					},
					size: '80%'
				}
			}
		},
		labels: ['Successful Pads', 'Other Pads'],
		legend: {
			position: 'bottom',
			fontFamily: 'Inter, sans-serif'
		}
	};


	function updateChart() {
	
		const filteredData = currentValue
			? landingPadsData.filter((pad) => pad.status.toLowerCase() === currentValue.toLowerCase())
			: landingPadsData;

		const totalLandingPads = filteredData.length;
		const successfulLandingPads = filteredData.filter((pad) => pad.successful_landings > 0).length;

		options.series = [successfulLandingPads, totalLandingPads - successfulLandingPads];
	}

	sharedState.subscribe((value) => {
		if(value === "null"){
			currentValue = null
		}else{
			currentValue = value;
		}
		updateChart();
	});
	
	updateChart();
</script>

<Card size="lg">
	<div class="flex w-full items-start justify-between">
		<div class="flex-col items-center">
			<div class="mb-1 flex items-center">
				<h5 class="me-1 text-xl font-bold leading-none text-gray-900 dark:text-white">
					Success Rate Chart
				</h5>
			</div>
		</div>
	</div>

	<Chart {options} class="py-6" />
</Card>

<script>
	import {onMount} from 'svelte'; 
	import Chart from 'svelte-frappe-charts';
	export let name;

	let chartRef;
	const color = ['#ebedf0', '#c0ddf9', '#73b3f3', '#3886e1', '#17459e'];
	const now = new Date();
	const start = (date) => new Date(date.getFullYear(), 0, 1); 
	const startDate = start(new Date());
	const endDate = new Date(startDate.getFullYear(), 11, 31);
	const generateRandomData = (startDate, endDate) => {
		const returnObject = {};
		let gauged = startDate.getTime() / 1000;
		const limitation = (endDate.getTime() / 1000);
		while ( gauged <= limitation ) {
			returnObject[gauged] = Math.floor(Math.random() * 100);
			gauged += Math.floor(Math.random() * 86400);
		}
		return returnObject;
	}
	onMount(async ()=> {
		const respose = await fetch('/api');
		const json = await respose.json();
		name = json.name;
	})

	let data = {
		dataPoints: generateRandomData(startDate, endDate),
		start: startDate,
		end: endDate,
  };
</script>

<main>
	{#if !!name == false}
	<p> Loading ...</p>
	{:else}
	<h1>Hello {name}!</h1>
	<h2>Monthly activity logs</h2>
	<div>
		<Chart data={data} colors={color} type="heatmap" bind:this={chartRef} />
	</div>
	{/if}

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	div {
		display: inline-block;
	}

	h1 {
		display: inline-block;
		color: #0000ff;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>

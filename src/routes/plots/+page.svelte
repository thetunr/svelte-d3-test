<script>
	import LinePlot from './LinePlot.svelte';
	import CirclePlot from './CirclePlot.svelte';
	import CircleXYPlot from './CircleXYPlot/CircleXYPlot.svelte';
	import ContourPlot from './ContourPlot.svelte';
</script>

<svelte:head>
	<title>Plots</title>
	<meta name="description" content="Test Plots" />
</svelte:head>

<div class="text-column">
	<h1 class="chart-header">Line Plot</h1>
	<LinePlot />
	<!-- <CirclePlot /> -->
	<h1 class="chart-header">Dot Plot</h1>
	<CircleXYPlot />
	<div class="chart-header">
		<h1>Contour Plot</h1>
		<h2>(Static)</h2>
	</div>
	<ContourPlot />
</div>

<style>
	.chart-header {
		margin-top: 1.5em;
		text-align: center;
	}
</style>

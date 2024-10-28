<script>
	import LinePlot from './LinePlot.svelte';
	import CircleXYPlot from './CircleXYPlot/CircleXYPlot.svelte';
	import ContourPlot from './ContourPlot.svelte';
	import BouquetPlot from './BouquetPlot.svelte';
	import Scrollable from './Scrollable.svelte';
</script>

<svelte:head>
	<title>Plots</title>
	<meta name="description" content="Test Plots" />
</svelte:head>

<div class="text-column">
	<h1 class="chart-header">Line Plot</h1>
	<LinePlot />
	<h1 class="chart-header">Dot Plot</h1>
	<CircleXYPlot />
	<div class="chart-header">
		<h1>Contour Plot</h1>
		<h2>(Static)</h2>
	</div>
	<ContourPlot />
	<h1 class="chart-header">Bouquet Plot</h1>
	<BouquetPlot />
	<h1 class="chart-header">Scrollable</h1>
	<Scrollable />
</div>

<style>
	.chart-header {
		margin-top: 1.5em;
		text-align: center;
	}
</style>

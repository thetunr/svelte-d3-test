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
	<h1>Example Plots</h1>
	<LinePlot />
	<CirclePlot />
	<CircleXYPlot />
	<ContourPlot />
</div>

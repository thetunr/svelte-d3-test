<script>
	import * as d3 from 'd3';

	// export let data;
	let data = [3, 4, 5, 7, 3, 5, 7, 4, 2, 4, 6, 7, 3];

	let width = 640;
	let height = 400;
	let marginTop = 20;
	let marginRight = 30;
	let marginBottom = 30;
	let marginLeft = 30;

	let gx;
	let gy;

	$: x = d3.scaleLinear([0, data.length - 1], [marginLeft, width - marginRight]);
	$: y = d3.scaleLinear(d3.extent(data), [height - marginBottom, marginTop]);
	$: line = d3.line((d, i) => x(i), y);
	$: d3.select(gy).call(d3.axisLeft(y));
	$: d3.select(gx).call(d3.axisBottom(x));

	let hoveredData;
	let tooltipVisible = false;
	let tooltipX;
	let tooltipY;
	let tooltipValue;

	function focusPoint(x, y, value) {
		tooltipVisible = true;
		tooltipX = x + 20;
		tooltipY = y + 35;
		tooltipValue = value;
	}
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	class="chart-container"
	on:mouseleave={() => {
		hoveredData = null;
	}}
>
	<svg {width} {height}>
		<g bind:this={gx} transform="translate(0,{height - marginBottom})" />
		<g bind:this={gy} transform="translate({marginLeft},0)" />
		<path fill="none" stroke="currentColor" stroke-width="1.5" d={line(data)} />
		<g fill="white" stroke="currentColor" stroke-width="1.5">
			{#each data as d, i}
				<!-- svelte-ignore a11y-mouse-events-have-key-events -->
				<circle
					key={i}
					cx={x(i)}
					cy={y(d)}
					r={hoveredData && hoveredData == i ? '10' : '5'}
					opacity={hoveredData ? (hoveredData == i ? '1' : '.3') : '1'}
					on:mouseover={() => {
						focusPoint(x(i), y(d), d);
						hoveredData = i;
					}}
					on:mouseleave={() => {
						tooltipVisible = false;
						hoveredData = null;
					}}
				/>
			{/each}
		</g>

		{#if tooltipVisible}
			<text x={tooltipX} y={tooltipY} dy="-1em" class="tooltip">
				{tooltipValue}
			</text>
		{/if}
	</svg>
</div>

<style>
	.chart-container {
		margin: auto;
	}

	circle {
		transition:
			r 300ms ease,
			opacity 300ms ease;
		cursor: pointer;
	}

	.tooltip {
		transition: opacity 300ms ease;
		font-size: 12;
		fill: black;
		text-anchor: middle;
	}
</style>

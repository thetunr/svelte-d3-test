<script>
	import * as d3 from 'd3';
	import data from '../../../data/data1.js';

	let width = 400;
	let height = 400;

	const margin = { top: 20, right: 40, left: 0, bottom: 20 };

	import { scaleLinear } from 'd3-scale';
	$: xScale = scaleLinear()
		.domain([0, 100])
		.range([0, width - margin.left - margin.right]);

	import { max } from 'd3-array';
	const yScale = scaleLinear()
		.domain([0, max(data, (d) => d.hours)])
		.range([height - margin.top - margin.bottom, 0]);

	import AxisX from './AxisX.svelte';
	import AxisY from './AxisY.svelte';
	import Tooltip from './Tooltip.svelte';

	let hoveredData;
</script>

<h1>Hours Studied vs. Score</h1>
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
	class="chart-container"
	bind:clientWidth={width}
	on:mouseleave={() => {
		hoveredData = null;
	}}
>
	<svg {width} {height}>
		<AxisX {height} {xScale} {margin} />
		<AxisY {width} {yScale} {margin} />
		<g class="circles" transform="translate({margin.left} {margin.top})">
			{#each data.sort((a, b) => a.score - b.score) as student}
				<circle
					cx={xScale(student.score)}
					cy={yScale(student.hours)}
					r={hoveredData && hoveredData == student ? '20' : '10'}
					opacity={hoveredData ? (hoveredData == student ? '1' : '.3') : '1'}
					fill="purple"
					stroke="black"
					on:mouseover={() => {
						hoveredData = student;
					}}
					on:focus={() => {
						hoveredData = student;
					}}
					tabIndex="0"
				/>
			{/each}
		</g>
	</svg>
	{#if hoveredData}
		<Tooltip data={hoveredData} {xScale} {yScale} />
	{/if}
</div>

<style>
	circle {
		transition:
			r 300ms ease,
			opacity 300ms ease;
		cursor: pointer;
	}

	circle:focus {
		outline: none;
	}

	h1 {
		font-size: 1.5rem;
		font-weight: 600;
		margin-bottom: 0.5rem;
	}
</style>

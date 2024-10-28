<script>
	import data from '../../data/data2.js';
	import { scaleLinear } from 'd3-scale';
	import { extent } from 'd3-array';
	import { draw, fade } from 'svelte/transition';
	import { onMount } from 'svelte';

	const width = 800;
	const height = 600;
	const margins = { top: 50, right: 100, bottom: 300, left: 100 };

	const xScale = scaleLinear()
		.domain([0, data.length])
		.range([margins.left, width - margins.right]);
	const yScale = scaleLinear()
		.domain(extent(data.map((d) => d.height)))
		.range([height - margins.bottom, margins.top]);

	const sizeScale = scaleLinear()
		.domain(extent(data.map((d) => d.value)))
		.range([20, 60]);
	const colorScale = scaleLinear()
		.domain(extent(data.map((d) => d.value)))
		.range(['#f3e2e1', '#a11a11']);

	const curvyLine = (x1, y1, x2, y2) => {
		return `M ${x1} ${y1} C ${x1} ${(y2 - y1) / 2 + y1}, ${x2} ${(y2 - y1) / 2 + y1}, ${x2} ${y2}`;
	};

	let show = false;

	onMount(() => {
		show = true;
	});
</script>

<div>
	<svg {height} {width}>
		<radialGradient id="radialGradient" cx="90%" cy="10%">
			<stop stop-color="#fff" stop-opacity=".7" />
			<stop offset="100%" stop-color="#fff" stop-opacity="0" />
		</radialGradient>
		{#if show}
			<g>
				{#each data as point, i}
					<g>
						<path
							d={curvyLine(width / 2, height - 100, xScale(i), yScale(point.height))}
							stroke={colorScale(point.value)}
							stroke-width="14"
							fill="none"
							opacity=".7"
							transition:draw|global={{ duration: 1000, delay: i * 100 }}
						/>
						<circle
							cx={xScale(i)}
							cy={yScale(point.height)}
							r={sizeScale(point.value)}
							fill={colorScale(point.value)}
							transition:fade|global={{ duration: 1000, delay: i * 100 + 500 }}
						/>
						<circle
							cx={xScale(i)}
							cy={yScale(point.height)}
							r={sizeScale(point.value)}
							fill="url('#radialGradient')"
							transition:fade|global={{ duration: 1000, delay: i * 100 + 500 }}
						/>
						<g transform="translate({xScale(i)}, {yScale(point.height)})">
							<text dominant-baseline="middle" text-anchor="middle" class="label"
								>{point.label}</text
							>
						</g>
					</g>
				{/each}
			</g>
		{/if}
	</svg>
</div>

<style>
	.label {
		font-size: 0.7rem;
		fill: white;
		text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
		font-family: Futura;
	}
</style>

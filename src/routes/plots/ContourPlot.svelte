<!-- https://observablehq.com/@d3/density-contours -->
<script>
	import { onMount } from 'svelte';
	import * as d3 from 'd3';

	const width = 928;
	const height = 600;
	const marginTop = 40;
	const marginRight = 40;
	const marginBottom = 40;
	const marginLeft = 40;

	let svgContainer;
	let data = []; // Initialize data variable

	onMount(async () => {
		// Load the data
		data = await d3.tsv(
			'https://raw.githubusercontent.com/thetunr/svelte-d3-test/refs/heads/main/src/data/faithful.tsv'
		);
		console.log(window.location.pathname);

		// Create the horizontal and vertical scales.
		const x = d3
			.scaleLinear()
			.domain(d3.extent(data, (d) => +d.waiting)) // Ensure 'waiting' is treated as a number
			.nice()
			.rangeRound([marginLeft, width - marginRight]);

		const y = d3
			.scaleLinear()
			.domain(d3.extent(data, (d) => +d.eruptions)) // Ensure 'eruptions' is treated as a number
			.nice()
			.rangeRound([height - marginBottom, marginTop]);

		// Compute the density contours.
		const contours = d3
			.contourDensity()
			.x((d) => x(d.waiting))
			.y((d) => y(d.eruptions))
			.size([width, height])
			.bandwidth(30)
			.thresholds(30)(data);

		// Create the SVG container
		const svg = d3
			.select(svgContainer)
			.attr('width', width)
			.attr('height', height)
			.attr('viewBox', [0, 0, width, height])
			.attr('style', 'max-width: 100%; height: auto;');

		// Append the axes
		svg
			.append('g')
			.attr('transform', `translate(0,${height - marginBottom})`)
			.call(d3.axisBottom(x).tickSizeOuter(0))
			.call((g) => g.select('.domain').remove())
			.call((g) =>
				g
					.select('.tick:last-of-type text')
					.clone()
					.attr('y', -3)
					.attr('dy', null)
					.attr('font-weight', 'bold')
					.text('Idle (min.)')
			);

		svg
			.append('g')
			.attr('transform', `translate(${marginLeft},0)`)
			.call(d3.axisLeft(y).tickSizeOuter(0))
			.call((g) => g.select('.domain').remove())
			.call((g) =>
				g
					.select('.tick:last-of-type text')
					.clone()
					.attr('x', 3)
					.attr('text-anchor', 'start')
					.attr('font-weight', 'bold')
					.text('Erupting (min.)')
			);

		// Append the contours
		svg
			.append('g')
			.attr('fill', 'none')
			.attr('stroke', 'steelblue')
			.attr('stroke-linejoin', 'round')
			.selectAll()
			.data(contours)
			.join('path')
			.attr('stroke-width', (d, i) => (i % 5 ? 0.25 : 1))
			.attr('d', d3.geoPath());

		// Append dots
		svg
			.append('g')
			.attr('stroke', 'white')
			.selectAll()
			.data(data)
			.join('circle')
			.attr('cx', (d) => x(d.waiting))
			.attr('cy', (d) => y(d.eruptions))
			.attr('r', 2);
	});
</script>

<svg bind:this={svgContainer}></svg>

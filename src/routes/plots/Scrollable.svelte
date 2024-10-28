<script>
	import Scroller from '@sveltejs/svelte-scroller';
	import CirclePlot from './CirclePlot.svelte';

	let index, offset, progress;
</script>

<Scroller top={0.2} bottom={0.8} bind:index bind:offset bind:progress>
	<div slot="background" class="background">
		<p>
			This is the background content. It will stay fixed in place while the foreground scrolls over
			the top.
		</p>

		<p>Section {index + 1} is currently active.</p>
	</div>

	<div slot="foreground">
		<section>
			<p>This is the first section.</p>
			<CirclePlot />
		</section>

		<section>
			<p>This is the second section.</p>
			<CirclePlot />
		</section>

		<section>
			<p>This is the third section.</p>
			<CirclePlot />
		</section>
	</div>
</Scroller>

<style>
	.background {
		background-color: aliceblue;
	}

	section {
		height: 80vh;
	}
</style>

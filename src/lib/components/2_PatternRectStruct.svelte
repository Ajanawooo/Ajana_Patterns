<script>
	import chroma from "chroma-js";
	import Slider from "$lib/ui/Slider.svelte";


	let offset = $state(0);
	let offsetY = $state(123);
	let offset2 = $state(0);
	let strecken = $state(0);
	let strecken2 = $state(0);
	let stauchen = $state(0);
	let stauchen2 = $state(0);

	// Zweiter Offset für zusätzliche Zeilenverschiebung in X-Richtung
	let zeilenVerschiebungX = $derived(offset2 % 150);

	let verschiebungX = $derived(offset % 150);
	let verschiebungY = $derived(offsetY);
	// $inspect(verschiebungY);

	// Feste Hintergrundfarben - hell mit leichtem Kontrast
	let color1 = "#F5F5F5"; // Sehr hellgrau
	let color2 = "#E8E8E8"; // Hellgrau


</script>

<div class="svg-container">
	<svg viewBox="0 -450 1000 1000" class="svg-canvas">
		{#each Array(30) as _, j}
			<rect
				x="0"
				y={50 + (j - 15) * verschiebungY}
				width={1000}
				height={verschiebungY}
				fill={j % 2 ? color1 : color2}
			/>

			<g
				transform="translate({verschiebungX * (j - 0) -
					zeilenVerschiebungX}, {50 + (j - 15) * verschiebungY})"
			>
				{#each Array(30) as _, i}
					<polygon
						class="triangle1"
						transform="translate({(i - 15) * 150}, 0)"
						points="0 0, {-50 -stauchen} 0, 0 {-100 - strecken}"
						fill="#2A2A2A"
					/>
					<polygon
						transform="translate({(i - 15) * 150}, 0) rotate(90)"
						points="0 0, {-50 - strecken2} 0, 0 {-100 - stauchen2}"
						fill="#404040"
					/>
				{/each}
			</g>
			<g
				transform="translate({verschiebungX * (j - 0) +
					zeilenVerschiebungX}, {50 +
					(j - 15) * verschiebungY})"
			>
				{#each Array(30) as _, i}
					<polygon
						class="triangle3"
						transform="translate({50 + (i - 15) * 150}, 0)"
						points="0 0, {100 + stauchen2} 0, 0 {50 + strecken2}"
						fill="#1A1A1A"
					/>
					<polygon
						class="triangle2"
						transform="translate({50 +
							(i - 15) * 150}, 0) rotate(90)"
						points="0 0, {100 + strecken} 0, 0 {50 + stauchen}"
						fill="#555555"
					/>
				{/each}
			</g>
		{/each}
	</svg>
</div>

<div class="sidebar-right">
	<Slider
		bind:value={offsetY}
		min={36}
		max={300}
		step={1}
		label="Y Verschiebung"
	/>

	<Slider
		bind:value={offset2}
		min={0}
		max={300}
		step={1}
		label="X Verschiebung"
		
	/>


	<Slider
		bind:value={strecken}
		min={-100}
		max={25}
		step={1}
		label="Vertikal Strecken Links"
		snapValues={[-100, -75, -50, -25, 0, 25]}
	/>
		<Slider
		bind:value={strecken2}
		min={-50}
		max={75}
		step={1}
		label="Vertikal Strecken Rechts"
		snapValues={[-100, -75, -50, -25, 0, 25, 50]}
	/>
	<Slider
		bind:value={stauchen}
		min={-50}
		max={100}
		step={1}
		label="Horizontal Strecken Links"
		snapValues={[-50, -25, 0, 25, 50, 75, 100]}
	/>

	<Slider
		bind:value={stauchen2}
		min={-100}
		max={50}
		step={1}
		label="Horizontal Strecken Rechts"
		snapValues={[-50, -25, 0, 25, 50, 75, 100]}
	/>

</div>

<style>
	#control {
		display: flex;
		flex-direction: column;
		align-items: left;
		justify-content: top;
		gap: 15px;
		margin: 15px;

		min-width: 200px !important;
	}

	.label {
		font-size: 14px;
		font-weight: 500;
		margin-bottom: 5px;
	}
</style>

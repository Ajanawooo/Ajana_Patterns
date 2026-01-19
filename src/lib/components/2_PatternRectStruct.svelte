<script>
	import chroma from "chroma-js";
	import Slider from "$lib/ui/Slider.svelte";

	const squareCount = 10;
	const squareSize = 1000 / squareCount;

	let offset = $state(0);
	let offset2 = $state(0);
	let strecken = $state(0);
	let stauchen = $state(0);
	let strecken2 = $state(0);
	let stauchen2 = $state(0);

	// Zweiter Offset für zusätzliche Zeilenverschiebung in X-Richtung
	let zeilenVerschiebungX = $derived(offset2 % 150);

	let verschiebungX = $derived(offset % 150);
	let verschiebungY = $derived(calculateVerschiebungY(verschiebungX));
	// $inspect(verschiebungY);

	// berechne eine farbe abhängig von verschiebungY
	// verschiebungY ist zwischen ca 110 und 200, die farbe soll zwischen hellgrün (luminance 0.8) und dunkelgrün (luminance 0.3) wechseln
	let startColor1 = chroma("#74C69D"); // Helles Grün
	let endColor1 = chroma("#2D6A4F"); // Dunkleres Grün
	let color1 = $derived(
		chroma
			.scale([startColor1, endColor1])
			.mode("oklch")
			.domain([110, 200])(verschiebungY)
			.hex(),
	);

	let startColor2 = chroma("#2D6A4F"); // Dunkleres Grün (umgedreht)
	let endColor2 = chroma("#74C69D"); // Helles Grün (umgedreht)
	let color2 = $derived(
		chroma
			.scale([startColor2, endColor2])
			.mode("oklch")
			.domain([110, 200])(verschiebungY)
			.hex(),
	);

	function calculateVerschiebungY(verschiebungX) {
		let combinedVerschiebungX =
			(verschiebungX - zeilenVerschiebungX - strecken - strecken2 + 150) % 150;
		// verschiebungX sollte immer zwischen 0 und 150 liegen
		if (combinedVerschiebungX >= 0 && combinedVerschiebungX < 50 / 3) {
			return 123 + (combinedVerschiebungX * 1) / 2;
		} else if (
			combinedVerschiebungX >= 50 / 3 &&
			combinedVerschiebungX < 50
		) {
			return 100 + combinedVerschiebungX * 2;
		} else if (
			combinedVerschiebungX >= 50 &&
			combinedVerschiebungX < 83.3333
		) {
			return 200 - (combinedVerschiebungX - 50) * 2;
		} else if (
			combinedVerschiebungX >= 83.3333 &&
			combinedVerschiebungX < 127
		) {
			return 175 + combinedVerschiebungX * (-1 / 2);
		} else if (
			combinedVerschiebungX >= 127 &&
			combinedVerschiebungX < 150
		) {
			return 50 + (combinedVerschiebungX * 1) / 2;
		} else if (combinedVerschiebungX === 150) {
			return 125;
		}

		return 150;
	}

	function calculateSizeCords1(xi, yi) {}

	function calculateSizeCords2(xi, yi) {}
</script>

<div class="svg-container">
	<svg viewBox="0 -450 1000 1000" class="svg-canvas">
		{#each Array(11) as _, j}
			<rect
				x="0"
				y={50 + (j - 5) * verschiebungY}
				width={1000}
				height={verschiebungY}
				fill={j % 2 ? color1 : color2}
			/>

			<g
				transform="translate({verschiebungX * (j - 0) -
					zeilenVerschiebungX}, {50 + (j - 5) * verschiebungY})"
			>
				{#each Array(30) as _, i}
					<polygon
						class="triangle1"
						transform="translate({(i - 15) * 150}, 0)"
						points="0 0, {-50 -stauchen} 0, 0 {-100 - strecken}"
						fill="#FF6B35"
					/>
					<polygon
						transform="translate({(i - 15) * 150}, 0) rotate(90)"
						points="0 0, {-50 - strecken2} 0, 0 {-100 - stauchen2}"
						fill="#D84315"
					/>
				{/each}
			</g>
			<g
				transform="translate({verschiebungX * (j - 0)}, {50 +
					(j - 5) * verschiebungY})"
			>
				{#each Array(30) as _, i}
					<polygon
						class="triangle3"
						transform="translate({50 + (i - 15) * 150}, 0)"
						points="0 0, {100 + stauchen2} 0, 0 {50 + strecken2}"
						fill="#FF8C42"
					/>
					<polygon
						class="triangle2"
						transform="translate({50 +
							(i - 15) * 150}, 0) rotate(90)"
						points="0 0, {100 + strecken} 0, 0 {50 + stauchen}"
						fill="#E65100"
					/>
				{/each}
			</g>
		{/each}
	</svg>
</div>

<div id="control">
	<Slider
		bind:value={offset}
		min={0}
		max={300}
		step={1}
		label="Verschiebung X"
		
	/>

	<Slider
		bind:value={offset2}
		min={0}
		max={300}
		step={1}
		label="Zeilen Verschiebung"
		
	/>


	<Slider
		bind:value={strecken}
		min={-100}
		max={25}
		step={1}
		label="Strecken"
		snapValues={[-100, -75, -50, -25, 0, 25]}
	/>
	<Slider
		bind:value={stauchen}
		min={-50}
		max={50}
		step={1}
		label="Stauchen"
		snapValues={[-50, -25, 0, 25, 50]}
	/>
	<Slider
		bind:value={strecken2}
		min={-100}
		max={50}
		step={1}
		label="Strecken2"
		snapValues={[-100, -75, -50, -25, 0, 25, 50]}
	/>
	<Slider
		bind:value={stauchen2}
		min={-50}
		max={50}
		step={1}
		label="Stauchen2"
		snapValues={[-50, -25, 0, 25, 50]}
	/>



</div>

<style>
	#control {
		display: flex;
		flex-direction: column;
		align-items: left;
		justify-content: center;
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

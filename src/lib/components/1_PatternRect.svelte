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

	// Feste Hintergrundfarben - hell mit leichtem Kontrast
	let color1 = "#F5F5F5"; // Sehr hellgrau
	let color2 = "#E8E8E8"; // Hellgrau

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
						fill='#2A2A2A'
					/>
					<polygon
						transform="translate({(i - 15) * 150}, 0) rotate(90)"
						points="0 0, {-50 - strecken2} 0, 0 {-100 - stauchen2}"
						fill="#404040"
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
		bind:value={offset}
		min={0}
		max={300}
		step={1}
		label="Zeilenbewegung"
		
	/>

	<Slider
		bind:value={offset2}
		min={0}
		max={300}
		step={1}
		label="Gruppenbewegung"
		
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

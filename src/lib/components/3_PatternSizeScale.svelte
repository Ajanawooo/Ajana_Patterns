<!-- 
  - HTML und Svelte Code zeigen + Funktionsaufrufe
  - Zeige wie das erste kleine Pattern beginnt (außerhalb der Schleife)

  - 2 Rechtecke in der Schleife und entsprechend der Indizes x, y, width und height anpassen
  - Funktionen müssen sie schreiben 
  - Kleiner Tipp mit data-x um zu sehen welche Indizes die Rects haben
-->

<script>
	import chroma from "chroma-js";

	const squareCount = 10;
	const squareSize = 1000 / squareCount;

	let offset = $state(0);
	let offset2 = $state(0);
	
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
		let combinedVerschiebungX = (verschiebungX - zeilenVerschiebungX + 150) % 150;
		// verschiebungX sollte immer zwischen 0 und 150 liegen
		if (combinedVerschiebungX >= 0 && combinedVerschiebungX < 50 / 3) {
			return 123 + (combinedVerschiebungX * 1) / 2;
		} else if (combinedVerschiebungX >= 50 / 3 && combinedVerschiebungX < 50) {
			return 100 + combinedVerschiebungX * 2;
		} else if (combinedVerschiebungX >= 50 && combinedVerschiebungX < 83.3333) {
			return 200 - (combinedVerschiebungX - 50) * 2;
		} else if (combinedVerschiebungX >= 83.3333 && combinedVerschiebungX < 127) {
			return 175 + combinedVerschiebungX * (-1 / 2);
		} else if (combinedVerschiebungX >= 127 && combinedVerschiebungX < 150) {
			return 50 + (combinedVerschiebungX * 1) / 2;
		} else if (combinedVerschiebungX === 150) {
			return 125;
		}

		return 150;
	}

	function calculateSizeCords1(xi, yi) {}

	function calculateSizeCords2(xi, yi) {}
</script>

<div id="control">
	<div>
		<input type="range" min="0" max="300" bind:value={offset} />
		<span>{offset}</span>
	</div>
	
	<div>
		<input type="range" min="0" max="300" bind:value={offset2} />
		<span>{offset2}</span>
	</div>
	
</div>

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
			<!-- <rect
				
				y={50 + (j-5) * verschiebungY}
				width={1000}
				height={1000 - (j < 5 ? 100 : 200)}
				fill="lightgreen"
			/> -->

			<g
				transform="translate({verschiebungX * (j - 0) - zeilenVerschiebungX}, {50 +
					(j - 5) * verschiebungY})"
			>
				{#each Array(30) as _, i}
					<polygon
						transform="translate({(i - 15) * 150}, 0)"
						points="0 0, -50 0, 0 -100"
						fill="#FF6B35"
					/>
					<polygon
						transform="translate({(i - 15) * 150}, 0) rotate(90)"
						points="0 0, -50 0, 0 -100"
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
						transform="translate({50 + (i - 15) * 150}, 0)"
						points="0 0, 100 0, 0 50"
						fill="#FF8C42"
					/>
					<polygon
						transform="translate({50 +
							(i - 15) * 150}, 0) rotate(90)"
						points="0 0, 100 0, 0 50"
						fill="#E65100"
					/>
				{/each}
			</g>
		{/each}

	</svg>
</div>

<style>
	#control {
		display: flex; 
		flex-direction: column;
		align-items: left;
		justify-content: center;
		gap: 15px;
		margin-left: 15px;
		
		min-width: 200px !important;
	}
</style>

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
	let verschiebungX = $derived(offset % 150);
	let verschiebungY = $derived(calculateVerschiebungY(verschiebungX));
// $inspect(verschiebungY);

	// berechne eine farbe abhängig von verschiebungY
	// verschiebungY ist zwischen ca 110 und 200, die farbe soll zwischen hellgrün (luminance 0.8) und dunkelgrün (luminance 0.3) wechseln
	let startColor1 = chroma("#74C69D"); // Helles Grün
	let endColor1 = chroma("#2D6A4F"); // Dunkleres Grün
	let color1 = $derived(chroma.scale([startColor1, endColor1]).mode('oklch').domain([110, 200])(verschiebungY).hex());

	let startColor2 = chroma("#2D6A4F"); // Dunkleres Grün (umgedreht)
	let endColor2 = chroma("#74C69D"); // Helles Grün (umgedreht)
	let color2 = $derived(chroma.scale([startColor2, endColor2]).mode('oklch').domain([110, 200])(verschiebungY).hex());


	function calculateVerschiebungY(verschiebungX) {
		// verschiebungX sollte immer zwischen 0 und 150 liegen
		if (verschiebungX >= 0 && verschiebungX < 50 / 3) {
			return 123 + verschiebungX * 1/2;
		} else if(verschiebungX >= 50/3 && verschiebungX < 50 ) {
			return 100 + verschiebungX * 2;
		} else if (verschiebungX >= 50 && verschiebungX < 83.3333) {
			return 200 - (verschiebungX - 50) * 2;
		} else if (verschiebungX >= 83.3333 && verschiebungX < 127) {
			return 175 + verschiebungX *(-1/2);
		} else if (verschiebungX >= 127 && verschiebungX < 150) {
			return 50 + verschiebungX * 1/2;
		} else if (verschiebungX === 150) {
			return 125;
		}

		return 150;
	}

	function calculateSizeCords1(xi, yi) {}

	function calculateSizeCords2(xi, yi) {}
</script>


<div id="control">
	<input type="range" min="0" max="300" bind:value={offset} />
	<label>{offset}</label>
</div>



<div class="svg-container">
	<svg viewBox="0 -450 1000 1000" class="svg-canvas">
		{#each Array(11) as _, j}
			<rect
				x="0"
				y={50 + (j-5) * verschiebungY}
				width={1000}
				height={verschiebungY}
				fill={j%2 ? color1 : color2}

			/>
			<!-- <rect
				
				y={50 + (j-5) * verschiebungY}
				width={1000}
				height={1000 - (j < 5 ? 100 : 200)}
				fill="lightgreen"
			/> -->

			<g transform="translate({verschiebungX * (j-0)}, {50 + (j-5) * verschiebungY})">
				{#each Array(20) as _, i}
					<polygon transform="translate({(i-10) * 150}, 0)" points="0 0, -50 0, 0 -100" fill="#FF6B35" />
					<polygon
						transform="translate({(i-10) * 150}, 0) rotate(90)"
						points="0 0, -50 0, 0 -100"
						fill="#D84315"
					/>
				{/each}
			</g>
			<g transform="translate({verschiebungX * (j-0)}, {50 + (j-5) * verschiebungY})">
				{#each Array(20) as _, i}
					<polygon
						transform="translate({50 + (i-10) * 150}, 0)"
						points="0 0, 100 0, 0 50"
						fill="#FF8C42"
					/>
					<polygon
						transform="translate({50 + (i-10) * 150}, 0) rotate(90)"
						points="0 0, 100 0, 0 50"
						fill="#E65100"
					/>
				{/each}
			</g>
		{/each}

		<!-- Pattern Schleife -->

		{#each Array(10) as _, row}
			{#each Array(10) as _, i}
				<!-- Rosa Dreieck -->
				<!-- <polygon 
					points="{squareSize * (i * 2) + (row % 2) * squareSize},{squareSize * 2 * row} {squareSize * (i * 2 + 1) + (row % 2) * squareSize},{squareSize * 2 * row} {squareSize * (i * 2) + (row % 2) * squareSize},{squareSize * 2 + squareSize * 2 * row}" 
					fill="hotpink" 
				/>
				 -->
				<!-- Gelbes Dreieck -->
				<!-- <polygon 
					points="{squareSize * (i * 2 + 1) + (row % 2) * squareSize},{squareSize * 2 * row} {squareSize * (i * 2 + 2) + (row % 2) * squareSize},{squareSize * 2 * row} {squareSize * (i * 2 + 1) + (row % 2) * squareSize},{squareSize * 2 + squareSize * 2 * row}" 
					fill="khaki" 
				/> -->
			{/each}
		{/each}
	</svg>
</div>

<style>
	#control {
		display: flex;
		align-items: center;
		gap: 15px;
		margin-left: 15px;
		min-width: 200px !important;	
		}
</style>
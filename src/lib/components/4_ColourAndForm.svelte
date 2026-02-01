<!-- 
  - Pattern 3: Farbsteuerung für Dreiecke
  - Hintergrund, Farbton (Hue), Sättigung und Helligkeit individuell steuerbar
-->

<script>
	import chroma from "chroma-js";
	import Slider from "$lib/ui/Slider.svelte";

	// Transformations-Parameter
	let offsetY = $state(123);
	let offset2 = $state(0);
	let verschiebungX = $state(0);
	let strecken = $state(0);
	let stauchen = $state(0);
	let strecken2 = $state(0);
	let stauchen2 = $state(0);

	// Derived values
	let verschiebungY = $derived(offsetY);
	let zeilenVerschiebungX = $derived(offset2 % 150);

	// Dreiecksfarben (Basis)
	const baseColor1 = "#FF6B35";
	const baseColor2 = "#D84315";
	const baseColor3 = "#FF8C42";
	const baseColor4 = "#E65100";

	// Hintergrundfarben (Basis)
	const baseBackgroundColor1 = "#6ab791";
	const baseBackgroundColor2 = "#38765a";

	// RGB Anpassungen für ALLE Dreiecksfarben (3 Slider für alle)
	let rAdjust = $state(0);
	let gAdjust = $state(0);
	let bAdjust = $state(0);

	// Hue-Wert für beide Hintergrundfarben (0-360 Grad)
	let hueBackground = $state(157); // Standard: Grünton

	// Aktive Palette verfolgen
	let activePalette = $state('original');

	// Farbpaletten Presets
	function setPalette1() {
		// Original Orange-Töne mit Grün-Hintergrund
		rAdjust = 191;
		gAdjust = 37;
		bAdjust = 42;
		hueBackground = 107; // Grün
		activePalette = 'palette1';
	}

	function setPalette2() {
		// Kühle Blau-Töne mit Blau-Hintergrund
		rAdjust = 80;
		gAdjust = 20;
		bAdjust = 100;
		hueBackground = 200; // Blau
		activePalette = 'palette2';
	}

	function setPalette3() {
		// Warme Rot-Gelb-Töne mit Rot-Hintergrund
		rAdjust = 140;
		gAdjust = 60;
		bAdjust = -50;
		hueBackground = 320; // Rot
		activePalette = 'palette3';
	}

	function setPalette4() {
		// Lila-Violett-Töne mit Lila-Hintergrund
		rAdjust = 100;
		gAdjust = -50;
		bAdjust = 120;
		hueBackground = 280; // Lila
		activePalette = 'palette4';
	}

	function setPalette5() {
		// Türkis-Cyan-Töne mit Türkis-Hintergrund
		rAdjust = -80;
		gAdjust = 80;
		bAdjust = 90;
		hueBackground = 180; // Türkis
		activePalette = 'palette5';
	}

	function resetToOriginal() {
		// Zurück zu den Ursprungswerten
		rAdjust = 0;
		gAdjust = 0;
		bAdjust = 0;
		hueBackground = 157; // Original Grün
		activePalette = 'original';
	}

	// Funktion zum Anpassen der RGB-Werte
	function adjustColor(baseColor, r, g, b) {
		const rgb = chroma(baseColor).rgb();
		return chroma([
			Math.max(0, Math.min(255, rgb[0] + r)),
			Math.max(0, Math.min(255, rgb[1] + g)),
			Math.max(0, Math.min(255, rgb[2] + b))
		]).hex();
	}

	// Reaktive Farben - alle werden mit denselben RGB-Werten angepasst
	let color1 = $derived(adjustColor(baseColor1, rAdjust, gAdjust, bAdjust));
	let color2 = $derived(adjustColor(baseColor2, rAdjust, gAdjust, bAdjust));
	let color3 = $derived(adjustColor(baseColor3, rAdjust, gAdjust, bAdjust));
	let color4 = $derived(adjustColor(baseColor4, rAdjust, gAdjust, bAdjust));

	// Reaktive Hintergrundfarben mit gemeinsamem Hue
	// Verhältnis von hell/dunkel bleibt gleich, nur Farbton ändert sich
	let backgroundColor1 = $derived(
		chroma(baseBackgroundColor1).set('hsl.h', hueBackground).hex()
	);
	let backgroundColor2 = $derived(
		chroma(baseBackgroundColor2).set('hsl.h', hueBackground).hex()
	);
</script>

<div class="svg-container">
	<svg viewBox="0 -450 1000 1000" class="svg-canvas">
		{#each Array(30) as _, j}
			<rect
				x="0"
				y={50 + (j - 15) * verschiebungY}
				width={1000}
				height={verschiebungY}
				fill={j % 2 ? backgroundColor1 : backgroundColor2}
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
						fill={color1}
					/>
					<polygon
						transform="translate({(i - 15) * 150}, 0) rotate(90)"
						points="0 0, {-50 - strecken2} 0, 0 {-100 - stauchen2}"
						fill={color2}
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
						fill={color3}
					/>
					<polygon
						class="triangle2"
						transform="translate({50 +
							(i - 15) * 150}, 0) rotate(90)"
						points="0 0, {100 + strecken} 0, 0 {50 + stauchen}"
						fill={color4}
					/>
				{/each}
			</g>
		{/each}
	</svg>
</div>


<div class="sidebar-right">
	
	<h3>Transformation</h3>
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
	
	<hr />
	<h3>Streckungen & Stauchungen</h3>
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
	<hr />
	<h3>Farbbeispiele</h3>
	<div class="palette-buttons">
		<button 
			onclick={resetToOriginal} 
			class="palette-btn palette-original" 
			class:active={activePalette === 'original'}
			aria-label="Original">
			Original
		</button>
		
		<button 
			onclick={setPalette2} 
			class="palette-btn palette-2" 
			class:active={activePalette === 'palette2'}
			aria-label="Kühle Palette">
			Beispiel 1
		</button>
		<button 
			onclick={setPalette1} 
			class="palette-btn palette-1" 
			class:active={activePalette === 'palette1'}
			aria-label="Original Palette">
			Beispiel 2
		</button>
		
	
	</div>
	<div class="palette-buttons">
			<button 
				onclick={setPalette3} 
				class="palette-btn palette-3" 
				class:active={activePalette === 'palette3'}
				aria-label="Warme Palette">
			Beispiel 3
		</button>
		<button 
			onclick={setPalette4} 
			class="palette-btn palette-4" 
			class:active={activePalette === 'palette4'}
			aria-label="Lila Palette">
			Beispiel 4
		</button>
		<button 
			onclick={setPalette5} 
			class="palette-btn palette-5" 
			class:active={activePalette === 'palette5'}
			aria-label="Türkis Palette">
			Beispiel 5
		</button>
		
	</div>

</div>

<style>
	.palette-buttons {
		display: flex;
		gap: 8px;
		margin-top: 8px;
	}

	.palette-btn {
		flex: 1;
		padding: 10px 16px;
		border: 2px solid #777;
		border-radius: 4px;
		background: #666;
		color: #fff;
		cursor: pointer;
		font-size: 0.75rem;
		transition: all 0.15s ease;
	}

	.palette-btn:hover {
		border-color: #fff;
		transform: scale(1.02);
	}

	.palette-btn:active {
		transform: scale(0.98);
	}

	.palette-btn.active {
		border-color: #fff;
		border-width: 3px;
		box-shadow: 0 0 8px rgba(255, 255, 255, 0.6);
	}

	/* Original - Orange Dreieck */
	.palette-original {
		background: #FF6B35;
	}

	/* Beispiel 1 - Blau-Töne */
	.palette-2 {
		background: #f68fc4;
	}

	/* Beispiel 2 - Intensiveres Orange */
	.palette-1 {
		background: #ffa64d;
	}

	/* Beispiel 3 - Warme Gelb-Töne */
	.palette-3 {
		background: #ffc329;
	}

	/* Beispiel 4 - Lila-Violett-Töne */
	.palette-4 {
		background: #ff69d4;
	}

	/* Beispiel 5 - Türkis-Cyan-Töne */
	.palette-5 {
		background: #74bbac;
	}
</style>


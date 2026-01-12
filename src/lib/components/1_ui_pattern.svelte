<script>
	import chroma from 'chroma-js';
	import Slider from '$lib/ui/Slider.svelte';
	import RangeSlider from '$lib/ui/RangeSlider.svelte';
	import Toggle from '$lib/ui/Toggle.svelte';
	import ColorPickerHSV from '$lib/ui/ColorPicker/ColorPickerHSV.svelte';
	import EditableColorPalette from '$lib/ui/EditableColorPalette.svelte';

	// Props für Triangle Pattern
	let { 
		scale = $bindable(1),
		rowOffset = $bindable(0),
		mirrorRows = $bindable(false),
		backgroundColor = $bindable('#000000'),
		polygon1Color = $bindable('#ff3e00'),
		polygon2Color = $bindable('#ff3e00')
	} = $props();

	let colors = $state([polygon1Color, polygon2Color]);
	let selectedColorIndex = $state(0);

	// Update polygon colors when colors array changes
	$effect(() => {
		polygon1Color = colors[0] || '#ff3e00';
		polygon2Color = colors[1] || '#ff3e00';
	});
</script>

<div class="sidebar">
	<h3 style="margin: 0 0 10px 0; font-size: 1rem; font-weight: 500;">Pattern Controls</h3>

	<Slider min={0.5} max={2} step={0.1} bind:value={scale} label="Skalierung" />

	<Slider min={-200} max={200} bind:value={rowOffset} label="Reihen-Versatz" />

	<Toggle bind:value={mirrorRows} label="Reihen spiegeln" />

	<div class="color-section">
		<h3 style="margin: 0 0 10px 0; font-size: 1rem; font-weight: 500;">
			Polygon Colors
		</h3>
		<EditableColorPalette
			bind:colors
			bind:selectedColorIndex
			width={310}
			height={310}
			swatchSize={30}
		/>
	</div>

	<div class="color-section">
		<h3 style="margin: 0 0 10px 0; font-size: 1rem; font-weight: 500;">
			Hintergrundfarbe
		</h3>
		<input type="color" bind:value={backgroundColor} style="width: 100%; height: 50px; border-radius: 8px; border: none; cursor: pointer;" />
	</div>
</div>

<style>
	div.sidebar {
		display: flex;
		flex-direction: column;
		list-style: none;
		padding: 20px;
		margin: 0;
		gap: 20px;
		justify-content: flex-start;
		overflow-y: auto;
		min-width: 350px;
		max-height: calc(100vh - 60px);
		/* Rechts am Bildschirm fixiert, unter dem Header */
		position: fixed;
		right: 0;
		top: 60px;
		bottom: 0;
		z-index: 1000;
		background: rgba(0, 0, 0, 0.8);
		backdrop-filter: blur(10px);
	}
</style>

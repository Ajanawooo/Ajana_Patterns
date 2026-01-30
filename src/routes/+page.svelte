<script>
	import { slide } from 'svelte/transition';
	import Header from '$lib/components/Header.svelte';
	import Pattern1 from '$lib/components/1_PatternRect.svelte';	
	import Pattern2 from '$lib/components/2_PatternRectStruct.svelte';	
	import Pattern3 from '$lib/components/3_Colour.svelte';
	import Pattern4 from '$lib/components/4_ColourAndForm.svelte';

	// UI Pattern for controls (von dozenten)
	import UiPattern from '$lib/components/1_ui_pattern.svelte';

	// Pattern-Daten
	const patterns = [
		{
			name: 'Original Pattern',
			description: 'Dreiecke mit spannender Verschiebung',
			component: Pattern1
		},
		{
			name: 'Form Verändern',
			description: 'Dreieck-Pattern mit Skalierung, Stauchung und Streckung',
			component: Pattern2
		},
		{
			name: 'Farbe Verändern',
			description: 'Dreieck-Pattern Farbe anpassen',
			component: Pattern3
		}
		,
		{
			name: 'Farbe & Form',
			description: 'Dreieck-Pattern Farbe und Form anpassen',
			component: Pattern4
		}
	];
	let selectedPattern = $state(0); // Standardmäßig Pattern 1 ausgewählt
	let SelectedPattern = $derived(patterns[selectedPattern].component);
</script>

<div class="app-container">
	<Header />
	<main class="app-main">
		<div class="sidebar-left">
			{#each patterns as pattern, index}
				<button
					class="sidebar-left-item"
					class:selected={selectedPattern === index}
					onclick={() => (selectedPattern = index)}
				>
					{pattern.name}
					{#if selectedPattern === index}
						<div transition:slide class="sidebar-left-description">
							{pattern.description}
						</div>
					{/if}
				</button>
			{/each}
		</div>

		<SelectedPattern />
	</main>
</div>
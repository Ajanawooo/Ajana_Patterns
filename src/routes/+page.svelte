<script>
	import { slide } from 'svelte/transition';
	import Header from '$lib/components/Header.svelte';
	import Pattern1 from '$lib/components/1_PatternRect.svelte';	
	import Pattern2 from '$lib/components/2_PatternRectStruct.svelte';	
	import Pattern3 from '$lib/components/3_PatternSizeScale.svelte';

	// UI Pattern for controls (von dozenten)
	import UiPattern from '$lib/components/1_ui_pattern.svelte';

	// Pattern-Daten
	const patterns = [
		{
			name: 'Original Pattern',
			description: 'Einfaches Rechteck-Pattern',
			component: Pattern1
		},
		{
			name: 'Pattern 2: Form Verändern',
			description: 'Rechteck-Pattern mit Struktur',
			component: Pattern2
		},
		{
			name: 'Pattern 3: Size Scale',
			description: 'Pattern mit Größen-Skalierung und Dreiecken',
			component: Pattern3
		}
	];

	let selectedPattern = $state(2); // Standardmäßig Pattern 3 ausgewählt
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

<style>
	.app-container {
		display: flex;
		flex-direction: column;
		height: 100vh;
		width: 100vw;
		background: #f0f0f0;
	}

	.app-main {
		display: flex;
		flex: 1;
		overflow: hidden;
		
	}

	.sidebar-left {
		width: 250px;
		background-color: #f0f0f0;
		padding: 0px;
		display: flex;
		flex-direction: column;
		gap: 0px;
		overflow-y: auto;
	}

	.sidebar-left-item {
		padding: 15px;
		background-color: white;
		border: 1px solid #ddd;
		border-radius: 0px;
		cursor: pointer;
		text-align: left;
		font-size: 14px;
		font-weight: 500;
		transition: all 0.2s ease;
	}

	.sidebar-left-item:hover {
		background-color: #f0f0f0;
		border-color: #999;
	}

	.sidebar-left-item.selected {
		background-color: #007acc;
		color: white;
		border-color: #005a9e;
	}

	.sidebar-left-description {
		margin-top: 10px;
		padding-top: 10px;
		border-top: 1px solid rgba(255, 255, 255, 0.3);
		font-size: 12px;
		font-weight: normal;
		opacity: 0.9;
	}
</style>

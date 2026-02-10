<script lang="ts">
	interface Props {
		name: string;
		size?: number | string;
		class?: string;
	}

	let { name, size = 24, class: className = '' }: Props = $props();

	// Import SVG icons dynamically
	const iconModules = import.meta.glob('$lib/icons/*.svg', { 
		query: '?raw',
		import: 'default',
		eager: true 
	}) as Record<string, string>;

	function getIconContent(): string {
		// Try to find the icon file
		const iconPath = `/src/lib/icons/${name}.svg`;
		const iconKey = Object.keys(iconModules).find(key => key.includes(`/${name}.svg`));
		
		if (iconKey && iconModules[iconKey]) {
			// Extract the SVG content without the wrapper
			const svgContent = iconModules[iconKey] as string;
			const match = svgContent.match(/<svg[^>]*>([\s\S]*?)<\/svg>/);
			return match ? match[1] : '';
		}
		
		// Fallback to file icon
		return '<path d="M13 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V9z"/><polyline points="13 2 13 9 20 9"/>';
	}

	const iconContent = getIconContent();
</script>

<svg
	width={size}
	height={size}
	viewBox="0 0 24 24"
	fill="none"
	stroke="currentColor"
	stroke-width="2"
	stroke-linecap="round"
	stroke-linejoin="round"
	class={className}
	aria-hidden="true"
>
	{@html iconContent}
</svg>

<script lang="ts">
	import LogoSvg from '$lib/assets/logo.svg';
	
	interface Props {
		size?: number;
		class?: string;
	}
	
	let { size = 32, class: className = '' }: Props = $props();
</script>

<img 
	src={LogoSvg} 
	alt="Glimpse Logo" 
	width={size} 
	height={size}
	class={className}
	style="display: block;"
/>

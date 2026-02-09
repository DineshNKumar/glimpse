<script lang="ts">
	import type { Snippet } from 'svelte';

	interface Props {
		variant?: 'primary' | 'success' | 'warning' | 'error' | 'info' | 'ai';
		class?: string;
		children: Snippet;
	}

	let {
		variant = 'primary',
		class: className = '',
		children
	}: Props = $props();

	const classes = $derived(() => {
		let base = 'badge';
		
		// Variant classes
		if (variant === 'primary') base += ' badge-primary';
		else if (variant === 'success') base += ' badge-success';
		else if (variant === 'warning') base += ' badge-warning';
		else if (variant === 'error') base += ' badge-error';
		else if (variant === 'info') base += ' badge-info';
		else if (variant === 'ai') base += ' badge-ai';
		
		// Custom classes
		if (className) base += ' ' + className;
		
		return base;
	});
</script>

<span class={classes()}>
	{@render children()}
</span>

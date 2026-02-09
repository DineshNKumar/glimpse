<script lang="ts">
	import type { Snippet } from 'svelte';

	interface Props {
		variant?: 'primary' | 'secondary' | 'ghost' | 'gradient' | 'ai';
		size?: 'sm' | 'base' | 'lg';
		disabled?: boolean;
		loading?: boolean;
		type?: 'button' | 'submit' | 'reset';
		onclick?: (e: MouseEvent) => void;
		class?: string;
		children: Snippet;
	}

	let {
		variant = 'primary',
		size = 'base',
		disabled = false,
		loading = false,
		type = 'button',
		onclick,
		class: className = '',
		children
	}: Props = $props();

	const classes = $derived(() => {
		let base = 'btn';
		
		// Variant classes
		if (variant === 'primary') base += ' btn-primary';
		else if (variant === 'secondary') base += ' btn-secondary';
		else if (variant === 'ghost') base += ' btn-ghost';
		else if (variant === 'gradient') base += ' btn-gradient';
		else if (variant === 'ai') base += ' btn-ai';
		
		// Size classes
		if (size === 'sm') base += ' btn-sm';
		else if (size === 'lg') base += ' btn-lg';
		
		// Custom classes
		if (className) base += ' ' + className;
		
		return base;
	});
</script>

<button
	{type}
	class={classes()}
	disabled={disabled || loading}
	onclick={onclick}
>
	{#if loading}
		<span class="spinner"></span>
	{/if}
	{@render children()}
</button>

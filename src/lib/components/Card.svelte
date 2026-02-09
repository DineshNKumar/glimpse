<script lang="ts">
	import type { Snippet } from 'svelte';

	interface Props {
		variant?: 'default' | 'glass' | 'elevated' | 'interactive' | 'static';
		hover?: boolean;
		onclick?: (e: MouseEvent) => void;
		class?: string;
		children: Snippet;
	}

	let {
		variant = 'default',
		hover = true,
		onclick,
		class: className = '',
		children
	}: Props = $props();

	const classes = $derived(() => {
		let base = 'card';
		
		// Variant classes
		if (variant === 'glass') base += ' card-glass';
		else if (variant === 'elevated') base += ' card-elevated';
		else if (variant === 'interactive') base += ' card-interactive';
		else if (variant === 'static') base += ' card-static';
		
		// Custom classes
		if (className) base += ' ' + className;
		
		return base;
	});

	const isClickable = !!onclick;
</script>

<div
	class={classes()}
	onclick={onclick}
	role={isClickable ? 'button' : undefined}
	tabindex={isClickable ? 0 : undefined}
	onkeydown={(e) => {
		if (isClickable && (e.key === 'Enter' || e.key === ' ')) {
			e.preventDefault();
			onclick?.(e as unknown as MouseEvent);
		}
	}}
>
	{@render children()}
</div>

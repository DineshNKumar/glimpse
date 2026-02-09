<script lang="ts">
	interface Props {
		value: number; // 0-100
		max?: number;
		variant?: 'default' | 'ai';
		size?: 'default' | 'large';
		indeterminate?: boolean;
		class?: string;
	}

	let {
		value = 0,
		max = 100,
		variant = 'default',
		size = 'default',
		indeterminate = false,
		class: className = ''
	}: Props = $props();

	const percentage = $derived(() => Math.min((value / max) * 100, 100));

	const containerClasses = $derived(() => {
		let base = 'progress-container';
		if (size === 'large') base += ' progress-large';
		if (indeterminate) base += ' progress-indeterminate';
		if (className) base += ' ' + className;
		return base;
	});

	const barClasses = $derived(() => {
		let base = 'progress-bar';
		if (variant === 'ai') base += ' progress-bar-ai';
		return base;
	});
</script>

<div class={containerClasses()}>
	{#if !indeterminate}
		<div 
			class={barClasses()} 
			style="width: {percentage()}%"
			role="progressbar"
			aria-valuenow={value}
			aria-valuemin={0}
			aria-valuemax={max}
		></div>
	{/if}
</div>

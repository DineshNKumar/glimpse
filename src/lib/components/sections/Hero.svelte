<script lang="ts">
	import { Icon, Badge } from '$lib/components';
	import type { Snippet } from 'svelte';
	
	interface Props {
		badge?: string;
		title: string;
		subtitle: string;
		primaryCta?: { text: string; href: string };
		secondaryCta?: { text: string; href: string };
		stats?: Array<{ value: string; label: string }>;
		visual?: Snippet;
	}
	
	let { 
		badge = '',
		title,
		subtitle,
		primaryCta,
		secondaryCta,
		stats = [],
		visual
	}: Props = $props();
</script>

<section class="hero">
	<div class="container">
		<div class="hero-grid">
			<!-- Left Content -->
			<div class="hero-content">
				{#if badge}
					<div class="hero-badge animate-fade-in">
						<Badge variant="primary">
							<Icon name="sparkles" size={16} />
							{badge}
						</Badge>
					</div>
				{/if}
				
				<h1 class="hero-title animate-fade-in">
					{@html title}
				</h1>
				
				<p class="hero-subtitle animate-fade-in">
					{subtitle}
				</p>

				{#if primaryCta || secondaryCta}
					<div class="hero-cta animate-fade-in">
						{#if primaryCta}
							<a href={primaryCta.href} class="btn btn-primary btn-lg">
								{primaryCta.text}
							</a>
						{/if}
						{#if secondaryCta}
							<a href={secondaryCta.href} class="btn btn-secondary btn-lg">
								{secondaryCta.text}
							</a>
						{/if}
					</div>
				{/if}

				{#if stats.length > 0}
					<div class="hero-stats animate-fade-in">
						{#each stats as stat}
							<div class="hero-stat">
								<div class="hero-stat-value">{stat.value}</div>
								<div class="hero-stat-label">{stat.label}</div>
							</div>
						{/each}
					</div>
				{/if}
			</div>

			<!-- Right Content (Slot for upload widget or visuals) -->
			<div class="hero-visual animate-fade-in">
				{#if visual}
					{@render visual()}
				{/if}
			</div>
		</div>
	</div>
</section>

<style>
	.hero {
		min-height: 100vh;
		display: flex;
		align-items: center;
		padding: var(--space-20) 0;
		background: linear-gradient(135deg, #f8f9ff 0%, #ffffff 100%);
	}

	.hero-grid {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: var(--space-16);
		align-items: center;
	}

	.hero-content {
		max-width: 600px;
	}

	.hero-badge {
		margin-bottom: var(--space-6);
	}

	.hero-title {
		font-size: var(--text-5xl);
		font-weight: var(--font-black);
		color: var(--gray-900);
		line-height: var(--leading-tight);
		margin-bottom: var(--space-6);
	}

	.hero-title :global(.gradient-text) {
		background: linear-gradient(135deg, var(--primary-500), var(--primary-700));
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.hero-subtitle {
		font-size: var(--text-xl);
		color: var(--gray-600);
		line-height: var(--leading-relaxed);
		margin-bottom: var(--space-8);
	}

	.hero-cta {
		display: flex;
		gap: var(--space-4);
		margin-bottom: var(--space-12);
	}

	.hero-stats {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
		gap: var(--space-6);
		padding-top: var(--space-8);
		border-top: 1px solid var(--gray-200);
	}

	.hero-stat {
		text-align: center;
	}

	.hero-stat-value {
		font-size: var(--text-2xl);
		font-weight: var(--font-bold);
		color: var(--primary-500);
		margin-bottom: var(--space-1);
	}

	.hero-stat-label {
		font-size: var(--text-sm);
		color: var(--gray-600);
	}

	.hero-visual {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	@media (max-width: 768px) {
		.hero {
			min-height: auto;
			padding: var(--space-12) 0 var(--space-16);
		}

		.hero-grid {
			grid-template-columns: 1fr;
			gap: var(--space-8);
		}

		.hero-title {
			font-size: var(--text-3xl);
		}

		.hero-subtitle {
			font-size: var(--text-base);
		}

		.hero-cta {
			flex-direction: column;
		}

		.hero-stats {
			grid-template-columns: repeat(2, 1fr);
		}
	}

	@keyframes fadeIn {
		from {
			opacity: 0;
			transform: translateY(20px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.animate-fade-in {
		animation: fadeIn 0.6s ease-out forwards;
	}

	.animate-fade-in:nth-child(1) { animation-delay: 0s; }
	.animate-fade-in:nth-child(2) { animation-delay: 0.1s; }
	.animate-fade-in:nth-child(3) { animation-delay: 0.2s; }
	.animate-fade-in:nth-child(4) { animation-delay: 0.3s; }
	.animate-fade-in:nth-child(5) { animation-delay: 0.4s; }
</style>

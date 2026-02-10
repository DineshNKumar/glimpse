<script lang="ts">
	import { Icon } from '$lib/components';

	interface Stat {
		icon: string;
		value: string;
		label: string;
		color?: string;
	}

	interface Props {
		heading?: string;
		stats?: Stat[];
		variant?: 'default' | 'colorful';
	}

	let {
		heading = "Trusted by thousands worldwide",
		stats = [
			{ icon: "file-text", value: "10M+", label: "Files Shared", color: "blue" },
			{ icon: "users", value: "500K+", label: "Active Users", color: "green" },
			{ icon: "activity", value: "99.9%", label: "Uptime", color: "orange" },
			{ icon: "shield", value: "256-bit", label: "Encryption", color: "purple" }
		],
		variant = 'colorful'
	}: Props = $props();
</script>

<div class="stats-section {variant}">
	<div class="stats-content">
		<div class="stats-header">
			<h3 class="stats-heading">{heading}</h3>
		</div>
		<div class="stats-row">
			{#each stats as stat, index}
				<div class="stat-item {stat.color}">
					<div class="stat-icon-wrapper">
						<Icon name={stat.icon} size={24} />
					</div>
					<div class="stat-content">
						<div class="stat-value">{stat.value}</div>
						<div class="stat-label">{stat.label}</div>
					</div>
				</div>
				{#if index < stats.length - 1}
					<div class="stat-divider"></div>
				{/if}
			{/each}
		</div>
	</div>
</div>

<style>
	/* Stats Section Container */
	.stats-section {
		margin-top: var(--space-16);
		padding: var(--space-12) 0;
	}

	.stats-section.colorful {
		background: linear-gradient(135deg, #f6f8ff 0%, #ffffff 100%);
		border-radius: var(--radius-3xl);
		padding: var(--space-12);
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
	}

	.stats-content {
		max-width: 1200px;
		margin: 0 auto;
	}

	.stats-header {
		text-align: center;
		margin-bottom: var(--space-10);
	}

	.stats-heading {
		font-size: var(--text-3xl);
		font-weight: var(--font-black);
		color: var(--gray-900);
		letter-spacing: -0.02em;
	}

	/* Horizontal Stats Row */
	.stats-row {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: var(--space-8);
		background: white;
		border-radius: var(--radius-2xl);
		padding: var(--space-8);
		box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
	}

	.stat-item {
		flex: 1;
		display: flex;
		align-items: center;
		gap: var(--space-4);
		padding: var(--space-4);
		border-radius: var(--radius-xl);
		transition: all var(--transition-base);
		cursor: default;
	}

	.stat-item:hover {
		transform: translateY(-2px);
		box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
	}

	/* Color Variants */
	.stat-item.blue .stat-icon-wrapper {
		background: linear-gradient(135deg, #3b82f6, #2563eb);
		box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
	}

	.stat-item.green .stat-icon-wrapper {
		background: linear-gradient(135deg, #10b981, #059669);
		box-shadow: 0 4px 12px rgba(16, 185, 129, 0.3);
	}

	.stat-item.orange .stat-icon-wrapper {
		background: linear-gradient(135deg, #f59e0b, #d97706);
		box-shadow: 0 4px 12px rgba(245, 158, 11, 0.3);
	}

	.stat-item.purple .stat-icon-wrapper {
		background: linear-gradient(135deg, #8b5cf6, #7c3aed);
		box-shadow: 0 4px 12px rgba(139, 92, 246, 0.3);
	}

	/* Icon Wrapper */
	.stat-icon-wrapper {
		flex-shrink: 0;
		width: 56px;
		height: 56px;
		display: flex;
		align-items: center;
		justify-content: center;
		border-radius: var(--radius-xl);
		color: white;
		transition: all var(--transition-base);
	}

	.stat-item:hover .stat-icon-wrapper {
		transform: scale(1.1) rotate(5deg);
	}

	/* Stat Content */
	.stat-content {
		flex: 1;
		min-width: 0;
	}

	.stat-value {
		font-size: var(--text-3xl);
		font-weight: var(--font-black);
		color: var(--gray-900);
		line-height: 1;
		margin-bottom: var(--space-1);
		letter-spacing: -0.02em;
	}

	.stat-label {
		font-size: var(--text-sm);
		color: var(--gray-600);
		font-weight: var(--font-semibold);
		line-height: 1.2;
	}

	/* Divider */
	.stat-divider {
		width: 1px;
		height: 48px;
		background: linear-gradient(to bottom, 
			transparent, 
			var(--gray-200) 20%, 
			var(--gray-200) 80%, 
			transparent);
		flex-shrink: 0;
	}

	/* Responsive Design */
	@media (max-width: 1024px) {
		.stats-row {
			gap: var(--space-4);
			padding: var(--space-6);
		}

		.stat-item {
			gap: var(--space-3);
			padding: var(--space-3);
		}

		.stat-icon-wrapper {
			width: 48px;
			height: 48px;
		}

		.stat-value {
			font-size: var(--text-2xl);
		}

		.stat-label {
			font-size: var(--text-xs);
		}

		.stat-divider {
			height: 40px;
		}
	}

	@media (max-width: 768px) {
		.stats-section.colorful {
			padding: var(--space-8);
			border-radius: var(--radius-2xl);
		}

		.stats-heading {
			font-size: var(--text-2xl);
		}

		.stats-row {
			flex-direction: column;
			gap: var(--space-6);
			padding: var(--space-6);
		}

		.stat-item {
			width: 100%;
			padding: var(--space-4);
		}

		.stat-divider {
			display: none;
		}

		.stat-icon-wrapper {
			width: 52px;
			height: 52px;
		}

		.stat-value {
			font-size: var(--text-2xl);
		}

		.stat-label {
			font-size: var(--text-sm);
		}
	}
</style>

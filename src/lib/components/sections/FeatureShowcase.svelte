<script lang="ts">
	import { Icon } from '$lib/components';

	interface FeatureItem {
		icon?: string;
		text: string;
	}

	interface Props {
		title: string;
		description: string;
		features: FeatureItem[];
		visualType?: 'preview' | 'security';
		reverse?: boolean;
	}

	let {
		title,
		description,
		features,
		visualType = 'preview',
		reverse = false
	}: Props = $props();
</script>

<div class="feature-showcase {reverse ? 'feature-showcase-reverse' : ''}">
	<div class="feature-showcase-visual">
		{#if visualType === 'preview'}
			<div class="preview-card">
				<div class="preview-header">
					<div class="preview-icon">
						<Icon name="file-text" size={32} />
					</div>
					<div class="badge badge-ai animate-pulse">
						<Icon name="sparkles" size={14} />
						AI Analyzed
					</div>
				</div>
				<div class="preview-content">
					<div class="preview-thumbnail">
						<span class="preview-icon-wrapper">
							<Icon name="image" size={48} />
						</span>
					</div>
					<div class="preview-lines">
						<div class="preview-line animate-shimmer" style="width: 90%"></div>
						<div class="preview-line animate-shimmer" style="width: 75%; animation-delay: 0.1s"></div>
						<div class="preview-line animate-shimmer" style="width: 95%; animation-delay: 0.2s"></div>
						<div class="preview-line animate-shimmer" style="width: 60%; animation-delay: 0.3s"></div>
					</div>
					<div class="preview-metadata">
						<div class="metadata-item">
							<Icon name="file-text" size={16} />
							<span>Document</span>
						</div>
						<div class="metadata-item">
							<Icon name="check-circle" size={16} />
							<span>Safe</span>
						</div>
						<div class="metadata-item">
							<Icon name="clock" size={16} />
							<span>2m ago</span>
						</div>
					</div>
				</div>
			</div>
		{:else if visualType === 'security'}
			<div class="security-card">
				<div class="security-shields">
					<div class="security-shield">
						<Icon name="shield" size={48} />
					</div>
					<div class="security-shield">
						<Icon name="lock" size={48} />
					</div>
					<div class="security-shield">
						<Icon name="check" size={48} />
					</div>
				</div>
				<div class="security-status">
					<div class="security-label">Security Status</div>
					<div class="security-value">Protected</div>
				</div>
			</div>
		{/if}
	</div>
	<div class="feature-showcase-content">
		<h3 class="feature-showcase-title">{title}</h3>
		<p class="feature-showcase-text">{description}</p>
		<div class="feature-list">
			{#each features as item}
				<div class="feature-item">
					{#if item.icon}
						<Icon name={item.icon} size={18} />
					{:else}
						<Icon name="check" size={18} />
					{/if}
					<span>{item.text}</span>
				</div>
			{/each}
		</div>
	</div>
</div>

<style>
	.feature-showcase {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: var(--space-12);
		align-items: center;
	}

	.feature-showcase-reverse {
		direction: rtl;
	}

	.feature-showcase-reverse > * {
		direction: ltr;
	}

	.feature-showcase-visual {
		padding: var(--space-8);
	}

	/* Preview Card Styles */
	.preview-card {
		background: white;
		border-radius: var(--radius-xl);
		padding: var(--space-6);
		box-shadow: var(--shadow-xl);
		border: 2px solid var(--gray-100);
		transition: all 0.3s ease;
	}

	.preview-card:hover {
		box-shadow: var(--shadow-ai);
		transform: translateY(-4px);
		border-color: var(--primary-200);
	}

	.preview-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: var(--space-6);
		padding-bottom: var(--space-4);
		border-bottom: 2px solid var(--gray-100);
	}

	.preview-icon {
		width: 48px;
		height: 48px;
		background: linear-gradient(135deg, var(--primary-500) 0%, var(--primary-600) 100%);
		border-radius: var(--radius-lg);
		display: flex;
		align-items: center;
		justify-content: center;
		color: white;
	}

	.preview-content {
		display: flex;
		flex-direction: column;
		gap: var(--space-4);
	}

	.preview-thumbnail {
		width: 100%;
		height: 120px;
		background: linear-gradient(135deg, var(--gray-100) 0%, var(--gray-50) 100%);
		border-radius: var(--radius-lg);
		display: flex;
		align-items: center;
		justify-content: center;
		color: var(--gray-400);
		margin-bottom: var(--space-2);
		position: relative;
		overflow: hidden;
	}

	.preview-thumbnail::before {
		content: '';
		position: absolute;
		inset: 0;
		background: linear-gradient(90deg, 
			transparent 0%, 
			rgba(139, 92, 246, 0.1) 50%, 
			transparent 100%);
		animation: shimmer 2s infinite;
	}

	.preview-icon-wrapper {
		position: relative;
		z-index: 1;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.preview-lines {
		display: flex;
		flex-direction: column;
		gap: var(--space-3);
	}

	.preview-line {
		height: 12px;
		background: linear-gradient(90deg, 
			var(--gray-200) 0%, 
			var(--gray-100) 50%, 
			var(--gray-200) 100%);
		background-size: 200% 100%;
		border-radius: var(--radius-md);
	}

	.animate-shimmer {
		animation: shimmer 2s ease-in-out infinite;
	}

	.preview-metadata {
		display: flex;
		gap: var(--space-4);
		padding-top: var(--space-4);
		border-top: 1px solid var(--gray-100);
	}

	.metadata-item {
		display: flex;
		align-items: center;
		gap: var(--space-2);
		font-size: var(--text-xs);
		color: var(--gray-600);
		padding: var(--space-2) var(--space-3);
		background: var(--gray-50);
		border-radius: var(--radius-md);
	}

	.metadata-item span {
		font-weight: var(--font-medium);
	}

	/* Security Card Styles */
	.security-card {
		background: white;
		border-radius: var(--radius-xl);
		padding: var(--space-8);
		box-shadow: var(--shadow-xl);
		text-align: center;
	}

	.security-shields {
		display: flex;
		justify-content: center;
		gap: var(--space-4);
		margin-bottom: var(--space-6);
	}

	.security-shield {
		width: 64px;
		height: 64px;
		background: var(--primary-50);
		border-radius: var(--radius-xl);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: var(--text-3xl);
		color: var(--primary-500);
	}

	.security-label {
		font-size: var(--text-sm);
		color: var(--gray-500);
		margin-bottom: var(--space-2);
	}

	.security-value {
		font-size: var(--text-2xl);
		font-weight: var(--font-bold);
		color: var(--success);
	}

	/* Feature Showcase Content */
	.feature-showcase-title {
		margin-bottom: var(--space-4);
		font-size: var(--text-3xl);
		font-weight: var(--font-bold);
		color: var(--gray-900);
	}

	.feature-showcase-text {
		color: var(--gray-600);
		margin-bottom: var(--space-6);
		font-size: var(--text-lg);
		line-height: var(--leading-relaxed);
	}

	.feature-list {
		display: flex;
		flex-direction: column;
		gap: var(--space-3);
		margin: 0;
		padding: 0;
	}

	.feature-item {
		display: flex;
		align-items: center;
		gap: var(--space-3);
		color: var(--gray-700);
		font-size: var(--text-base);
	}

	.feature-item :global(svg) {
		flex-shrink: 0;
		color: var(--primary-500);
	}

	.feature-item span {
		font-weight: var(--font-normal);
		line-height: 1.5;
	}

	/* Animations */
	@keyframes shimmer {
		0% {
			background-position: 200% 0;
		}
		100% {
			background-position: -200% 0;
		}
	}

	@keyframes pulse {
		0%, 100% {
			opacity: 1;
		}
		50% {
			opacity: 0.5;
		}
	}

	.animate-pulse {
		animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
	}

	/* Responsive */
	@media (max-width: 768px) {
		.feature-showcase {
			grid-template-columns: 1fr;
			gap: var(--space-8);
		}

		.feature-showcase-reverse {
			direction: ltr;
		}

		.feature-showcase-visual {
			padding: var(--space-4);
		}
	}
</style>

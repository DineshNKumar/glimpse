<script lang="ts">
	import { Icon } from '$lib/components';

	interface Feature {
		icon: string;
		title: string;
		description: string;
	}

	interface Props {
		features?: Feature[];
	}

	let {
		features = [
			{
				icon: "lock",
				title: "End-to-End Encrypted",
				description: "Your files are encrypted before upload"
			},
			{
				icon: "eye",
				title: "Visual Preview",
				description: "See thumbnails before downloading"
			},
			{
				icon: "robot",
				title: "AI Analysis",
				description: "Automatic content detection"
			},
			{
				icon: "zap",
				title: "Lightning Fast",
				description: "Optimized transfer speeds"
			}
		]
	}: Props = $props();
</script>

<div class="features-grid">
	{#each features as feature}
		<div class="feature-card">
			<div class="feature-icon">
				<Icon name={feature.icon} size={32} />
			</div>
			<h4 class="feature-card-title">{feature.title}</h4>
			<p class="feature-card-text">{feature.description}</p>
		</div>
	{/each}
</div>

<style>
	.features-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
		gap: var(--space-4);
		margin-top: var(--space-8);
	}

	.feature-card {
		background: white;
		padding: var(--space-6);
		border-radius: var(--radius-xl);
		text-align: center;
		border: 1px solid var(--gray-200);
		transition: all 0.3s ease;
	}

	.feature-card:hover {
		border-color: var(--primary-500);
		transform: translateY(-2px);
		box-shadow: var(--shadow-lg);
	}

	.feature-icon {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 64px;
		height: 64px;
		margin: 0 auto var(--space-4);
		background: linear-gradient(135deg, var(--primary-500) 0%, var(--primary-600) 100%);
		border-radius: var(--radius-xl);
		color: white;
	}

	.feature-card-title {
		font-size: var(--text-base);
		font-weight: var(--font-semibold);
		color: var(--gray-900);
		margin-bottom: var(--space-2);
	}

	.feature-card-text {
		font-size: var(--text-sm);
		color: var(--gray-600);
		margin: 0;
	}

	@media (max-width: 768px) {
		.features-grid {
			grid-template-columns: repeat(2, 1fr);
		}
	}
</style>

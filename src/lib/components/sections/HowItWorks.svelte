<script lang="ts">
	interface Step {
		number: number;
		title: string;
		description: string;
	}

	interface Props {
		title?: string;
		subtitle?: string;
		steps?: Step[];
	}

	let {
		title = "How Glimpse Works",
		subtitle = "Three simple steps to secure file sharing",
		steps = [
			{
				number: 1,
				title: "Upload Your Files",
				description: "Drag and drop or select files. Our AI automatically scans and prepares them for secure transfer."
			},
			{
				number: 2,
				title: "Generate Smart Link",
				description: "Get a secure, password-protected link with customizable expiration and download limits."
			},
			{
				number: 3,
				title: "Share with Confidence",
				description: "Recipients can preview before downloading. You get real-time analytics on views and downloads."
			}
		]
	}: Props = $props();
</script>

<section class="section" id="how-it-works">
	<div class="container">
		<div class="section-header">
			<h2 class="section-title">{title}</h2>
			<p class="section-subtitle">{subtitle}</p>
		</div>

		<div class="steps">
			{#each steps as step}
				<div class="step">
					<div class="step-number">{step.number}</div>
					<div class="step-content">
						<h3 class="step-title">{step.title}</h3>
						<p class="step-text">{step.description}</p>
					</div>
				</div>
			{/each}
		</div>
	</div>
</section>

<style>
	.section {
		padding: var(--space-20) 0;
	}

	.section-header {
		text-align: center;
		margin-bottom: var(--space-16);
		max-width: 700px;
		margin-left: auto;
		margin-right: auto;
	}

	.section-title {
		margin-bottom: var(--space-4);
		margin-top: var(--space-4);
	}

	.section-subtitle {
		font-size: var(--text-lg);
		color: var(--gray-600);
		margin: 0;
	}

	.steps {
		display: grid;
		gap: var(--space-12);
		max-width: 800px;
		margin: 0 auto;
	}

	.step {
		display: flex;
		gap: var(--space-6);
		align-items: flex-start;
	}

	.step-number {
		flex-shrink: 0;
		width: 56px;
		height: 56px;
		background: var(--bg-gradient-ai);
		color: white;
		border-radius: var(--radius-full);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: var(--text-2xl);
		font-weight: var(--font-bold);
		box-shadow: var(--shadow-ai);
	}

	.step-title {
		margin-bottom: var(--space-3);
		color: var(--gray-900);
		font-size: var(--text-xl);
		font-weight: var(--font-bold);
	}

	.step-text {
		color: var(--gray-600);
		margin: 0;
		line-height: var(--leading-relaxed);
	}
</style>

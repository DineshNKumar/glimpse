<script lang="ts">
	import { 
		Header, 
		Footer, 
		StatsSection, 
		FeaturesGrid, 
		HowItWorks, 
		FeatureShowcase,
		FileList,
		Icon 
	} from '$lib/components';
	import type { FileItem } from '$lib/components/transfer/FileList.svelte';

	// State management
	let files = $state<FileItem[]>([]);
	let isDragging = $state(false);
	let showAIFeatures = $state(false);

	// File upload handlers
	function handleDragOver(e: DragEvent) {
		e.preventDefault();
		isDragging = true;
	}

	function handleDragLeave(e: DragEvent) {
		e.preventDefault();
		isDragging = false;
	}

	function handleDrop(e: DragEvent) {
		e.preventDefault();
		isDragging = false;
		
		const droppedFiles = Array.from(e.dataTransfer?.files || []);
		handleFiles(droppedFiles);
	}

	function handleFileInput(e: Event) {
		const input = e.target as HTMLInputElement;
		const selectedFiles = Array.from(input.files || []);
		handleFiles(selectedFiles);
	}

	function handleFiles(newFiles: File[]) {
		const fileItems: FileItem[] = newFiles.map((file, index) => ({
			id: `${Date.now()}-${index}`,
			file,
			name: file.name,
			size: file.size,
			status: 'uploading',
			progress: 0
		}));

		files = [...files, ...fileItems];
		simulateUpload();
	}

	function simulateUpload() {
		const uploadingFiles = files.filter(f => f.status === 'uploading');
		
		uploadingFiles.forEach((fileItem) => {
			const interval = setInterval(() => {
				files = files.map(f => {
					if (f.id === fileItem.id) {
						const newProgress = Math.min((f.progress || 0) + 5, 100);
						if (newProgress >= 100) {
							clearInterval(interval);
							showAIFeatures = true;
							return { ...f, progress: 100, status: 'completed' as const };
						}
						return { ...f, progress: newProgress };
					}
					return f;
				});
			}, 100);
		});
	}

	function handleRemoveFile(fileId: string) {
		files = files.filter(f => f.id !== fileId);
		if (files.length === 0) {
			showAIFeatures = false;
		}
	}

	function handleClearAll() {
		files = [];
		showAIFeatures = false;
	}

	// Navigation handler
	function navigateToTransfer() {
		window.location.href = '/transfer/new';
	}

	// File input reference
	let fileInputRef: HTMLInputElement | undefined = $state();
</script>

<svelte:head>
	<title>Glimpse - See Before You Download</title>
	<meta name="description" content="Transfer files with intelligent previews, automatic virus scanning, and AI-powered content analysis. Share securely, download confidently." />
</svelte:head>

<!-- Header -->
<Header />

<!-- Hero Section with Upload -->
<section class="hero">
	<div class="container">
		<div class="hero-content">
			<div class="badge badge-ai animate-fade-in">
				<Icon name="sparkles" size={16} />
				AI-Powered File Intelligence
			</div>
			
			<h1 class="hero-title animate-fade-in">
				See before you <span class="gradient-text">download</span>
			</h1>
			
			<p class="hero-subtitle animate-fade-in">
				Transfer files with intelligent previews, automatic virus scanning, 
				and AI-powered content analysis. Share securely, download confidently.
			</p>

			<!-- Upload Zone Container -->
			<div class="upload-container animate-fade-in">
				<div 
					class="upload-zone {isDragging ? 'upload-zone-dragging' : ''} {files.length > 0 ? 'upload-zone-active' : ''}"
					role="button"
					tabindex="0"
					ondragover={handleDragOver}
					ondragleave={handleDragLeave}
					ondrop={handleDrop}
				>
					{#if files.length === 0}
						<div class="upload-placeholder">
							<div class="upload-icon">
								<Icon name="cloud-upload" size={64} />
							</div>
							<h3 class="upload-title">Drop files here or click to browse</h3>
							<p class="upload-text">
								Support for any file type • Up to 5GB per file • End-to-end encrypted
							</p>
							<input 
								type="file" 
								multiple 
								class="upload-input"
								onchange={handleFileInput}
								bind:this={fileInputRef}
							/>
							<button class="btn btn-primary btn-lg" onclick={() => fileInputRef?.click()}>
								<Icon name="folder-open" size={20} />
								<span>Choose Files</span>
							</button>
						</div>
					{:else}
						<FileList 
							{files} 
							onRemove={handleRemoveFile}
							onClearAll={handleClearAll}
						/>

						{#if showAIFeatures}
							<div class="ai-features">
								<div class="ai-feature">
									<span class="ai-icon"><Icon name="shield" size={20} /></span>
									<div>
										<div class="ai-feature-title">Security Scan Complete</div>
										<div class="ai-feature-text">No threats detected</div>
									</div>
								</div>
								<div class="ai-feature">
									<span class="ai-icon"><Icon name="search" size={20} /></span>
									<div>
										<div class="ai-feature-title">Content Analysis</div>
										<div class="ai-feature-text">Preview available for recipients</div>
									</div>
								</div>
								<div class="ai-feature">
									<span class="ai-icon"><Icon name="zap" size={20} /></span>
									<div>
										<div class="ai-feature-title">Smart Compression</div>
										<div class="ai-feature-text">Optimized for faster transfer</div>
									</div>
								</div>
							</div>

							<div class="upload-actions">
								<button class="btn btn-ai btn-lg" style="width: 100%;" onclick={navigateToTransfer}>
									<span>Generate Share Link</span>
									<Icon name="sparkles" size={20} />
								</button>
							</div>
						{/if}
					{/if}
				</div>

				<!-- Quick Features Grid -->
				<FeaturesGrid />
			</div>

			<!-- Stats Section -->
			<StatsSection />
		</div>
	</div>
</section>

<!-- How It Works Section -->
<HowItWorks />

<!-- Features Section with Showcases -->
<section class="section section-features" id="features">
	<div class="container">
		<div class="section-header">
			<div class="badge badge-ai">Powered by AI</div>
			<h2 class="section-title">Intelligence Built In</h2>
			<p class="section-subtitle">
				Advanced features that make file sharing smarter and safer
			</p>
		</div>

		<div class="features-showcase">
			<FeatureShowcase
				title="Smart Content Preview"
				description="AI-powered previews for documents, images, videos, and more. Recipients see what they're downloading before committing bandwidth."
				visualType="preview"
				features={[
					{ text: "Auto-generated thumbnails" },
					{ text: "Document text extraction" },
					{ text: "Metadata analysis" },
					{ text: "Content safety scoring" }
				]}
			/>

			<FeatureShowcase
				title="Multi-Layer Security"
				description="Enterprise-grade security with automatic virus scanning, end-to-end encryption, and access controls."
				visualType="security"
				reverse={true}
				features={[
					{ text: "Real-time virus scanning" },
					{ text: "256-bit AES encryption" },
					{ text: "Password protection" },
					{ text: "Expiring links" }
				]}
			/>
		</div>
	</div>
</section>

<!-- Footer -->
<Footer />

<style>
	/* Hero Section */
	.hero {
		background: var(--bg-gradient-hero);
		padding: var(--space-16) 0 var(--space-24);
		position: relative;
		overflow: hidden;
		min-height: 100vh;
		display: flex;
		align-items: center;
	}

	.hero-content {
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
		max-width: 900px;
		margin: 0 auto;
	}

	.hero-title {
		margin-top: var(--space-6);
		margin-bottom: var(--space-6);
		max-width: 800px;
		font-size: var(--text-5xl);
		font-weight: var(--font-black);
		color: var(--gray-900);
	}

	.gradient-text {
		background: var(--bg-gradient-ai);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.hero-subtitle {
		font-size: var(--text-xl);
		color: var(--gray-600);
		max-width: 600px;
		margin-bottom: var(--space-12);
		line-height: var(--leading-relaxed);
	}

	/* Upload Container Styles */
	.upload-container {
		width: 100%;
		max-width: 700px;
	}

	.upload-zone {
		background: white;
		border: 2px dashed var(--gray-300);
		border-radius: var(--radius-2xl);
		padding: var(--space-12);
		transition: all var(--transition-base);
		position: relative;
		min-height: 300px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.upload-zone-dragging {
		border-color: var(--primary-500);
		background: var(--primary-50);
		transform: scale(1.02);
	}

	.upload-zone-active {
		border-style: solid;
		border-color: var(--primary-500);
	}

	.upload-placeholder {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: var(--space-4);
		text-align: center;
		width: 100%;
	}

	.upload-icon {
		color: var(--gray-400);
		margin-bottom: var(--space-4);
	}

	.upload-title {
		font-size: var(--text-2xl);
		font-weight: var(--font-bold);
		color: var(--gray-900);
		margin-bottom: var(--space-2);
	}

	.upload-text {
		color: var(--gray-500);
		font-size: var(--text-sm);
		margin-bottom: var(--space-6);
	}

	.upload-input {
		position: absolute;
		width: 0;
		height: 0;
		opacity: 0;
		pointer-events: none;
	}

	/* AI Features Section */
	.ai-features {
		display: flex;
		flex-direction: column;
		gap: var(--space-3);
		margin-top: var(--space-6);
		padding: var(--space-6);
		background: var(--primary-50);
		border-radius: var(--radius-lg);
	}

	.ai-feature {
		display: flex;
		align-items: flex-start;
		gap: var(--space-3);
	}

	.ai-icon {
		font-size: var(--text-2xl);
		flex-shrink: 0;
		color: var(--primary-500);
	}

	.ai-feature-title {
		font-weight: var(--font-semibold);
		color: var(--gray-900);
		margin-bottom: var(--space-1);
	}

	.ai-feature-text {
		font-size: var(--text-sm);
		color: var(--gray-600);
	}

	.upload-actions {
		margin-top: var(--space-6);
	}

	/* Section Styles */
	.section {
		padding: var(--space-20) 0;
	}

	.section-features {
		background: white;
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
		font-size: var(--text-4xl);
		font-weight: var(--font-black);
		color: var(--gray-900);
	}

	.section-subtitle {
		font-size: var(--text-lg);
		color: var(--gray-600);
		margin: 0;
	}

	.features-showcase {
		display: flex;
		flex-direction: column;
		gap: var(--space-20);
	}

	/* Responsive */
	@media (max-width: 768px) {
		.upload-zone {
			padding: var(--space-6);
		}

		.section {
			padding: var(--space-12) 0;
		}

		.features-showcase {
			gap: var(--space-12);
		}
	}
</style>

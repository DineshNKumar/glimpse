<script lang="ts">
	import { onMount } from 'svelte';

	let isDragging = $state(false);
	let uploadedFiles = $state<File[]>([]);
	let uploadProgress = $state(0);
	let isUploading = $state(false);
	let showAIFeatures = $state(false);

	// Dark mode toggle
	let isDarkMode = $state(false);

	function toggleDarkMode() {
		isDarkMode = !isDarkMode;
		if (isDarkMode) {
			document.documentElement.classList.add('dark');
		} else {
			document.documentElement.classList.remove('dark');
		}
	}

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
		
		const files = Array.from(e.dataTransfer?.files || []);
		handleFiles(files);
	}

	function handleFileInput(e: Event) {
		const input = e.target as HTMLInputElement;
		const files = Array.from(input.files || []);
		handleFiles(files);
	}

	function handleFiles(files: File[]) {
		uploadedFiles = [...uploadedFiles, ...files];
		simulateUpload();
	}

	function simulateUpload() {
		isUploading = true;
		uploadProgress = 0;
		
		const interval = setInterval(() => {
			uploadProgress += 5;
			if (uploadProgress >= 100) {
				clearInterval(interval);
				isUploading = false;
				showAIFeatures = true;
			}
		}, 100);
	}

	function removeFile(index: number) {
		uploadedFiles = uploadedFiles.filter((_, i) => i !== index);
		if (uploadedFiles.length === 0) {
			showAIFeatures = false;
			uploadProgress = 0;
		}
	}

	function formatFileSize(bytes: number): string {
		if (bytes === 0) return '0 Bytes';
		const k = 1024;
		const sizes = ['Bytes', 'KB', 'MB', 'GB'];
		const i = Math.floor(Math.log(bytes) / Math.log(k));
		return Math.round(bytes / Math.pow(k, i) * 100) / 100 + ' ' + sizes[i];
	}

	function getFileIcon(fileName: string): string {
		const ext = fileName.split('.').pop()?.toLowerCase();
		const icons: Record<string, string> = {
			'pdf': '📄',
			'doc': '📝',
			'docx': '📝',
			'xls': '📊',
			'xlsx': '📊',
			'ppt': '📽️',
			'pptx': '📽️',
			'jpg': '🖼️',
			'jpeg': '🖼️',
			'png': '🖼️',
			'gif': '🖼️',
			'svg': '🎨',
			'mp4': '🎬',
			'mp3': '🎵',
			'zip': '📦',
			'rar': '📦',
			'txt': '📃'
		};
		return icons[ext || ''] || '📁';
	}

	onMount(() => {
		// Check for saved dark mode preference
		if (localStorage.getItem('darkMode') === 'true') {
			toggleDarkMode();
		}
	});
</script>

<!-- Navigation -->
<nav class="nav">
	<div class="container">
		<div class="nav-content">
			<div class="nav-brand">
				<div class="logo">
					<svg width="32" height="32" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
						<rect width="32" height="32" rx="8" fill="url(#logo-gradient)"/>
						<path d="M16 8L8 16L16 24L24 16L16 8Z" fill="white" opacity="0.9"/>
						<circle cx="16" cy="16" r="3" fill="white"/>
						<defs>
							<linearGradient id="logo-gradient" x1="0" y1="0" x2="32" y2="32" gradientUnits="userSpaceOnUse">
								<stop stop-color="#8b5cf6"/>
								<stop offset="1" stop-color="#6366f1"/>
							</linearGradient>
						</defs>
					</svg>
					<span class="logo-text">Glimpse</span>
				</div>
			</div>
			
			<div class="nav-links">
				<a href="#features" class="nav-link">Features</a>
				<a href="#how-it-works" class="nav-link hidden-mobile">How it works</a>
				<a href="#pricing" class="nav-link hidden-mobile">Pricing</a>
				<button class="btn btn-secondary btn-sm" onclick={toggleDarkMode}>
					{isDarkMode ? '☀️' : '🌙'}
				</button>
				<button class="btn btn-primary btn-sm">Sign In</button>
			</div>
		</div>
	</div>
</nav>

<!-- Hero Section -->
<section class="hero">
	<div class="container">
		<div class="hero-content">
			<div class="badge badge-ai animate-fade-in">
				✨ AI-Powered File Intelligence
			</div>
			
			<h1 class="hero-title animate-fade-in">
				See before you <span class="gradient-text">download</span>
			</h1>
			
			<p class="hero-subtitle animate-fade-in">
				Transfer files with intelligent previews, automatic virus scanning, 
				and AI-powered content analysis. Share securely, download confidently.
			</p>

			<!-- Upload Zone -->
			<div class="upload-container animate-fade-in">
				<div 
					class="upload-zone {isDragging ? 'upload-zone-dragging' : ''} {uploadedFiles.length > 0 ? 'upload-zone-active' : ''}"
					ondragover={handleDragOver}
					ondragleave={handleDragLeave}
					ondrop={handleDrop}
				>
					{#if uploadedFiles.length === 0}
						<div class="upload-placeholder">
							<div class="upload-icon">
								<svg width="64" height="64" viewBox="0 0 64 64" fill="none">
									<circle cx="32" cy="32" r="28" stroke="currentColor" stroke-width="2" stroke-dasharray="4 4" opacity="0.3"/>
									<path d="M32 20V44M20 32H44" stroke="currentColor" stroke-width="3" stroke-linecap="round"/>
								</svg>
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
							/>
							<button class="btn btn-primary btn-lg">
								<span>Choose Files</span>
								<span>📁</span>
							</button>
						</div>
					{:else}
						<div class="files-list">
							<div class="files-header">
								<h3 class="files-title">
									{uploadedFiles.length} {uploadedFiles.length === 1 ? 'file' : 'files'} selected
								</h3>
								<button class="btn btn-sm btn-secondary" onclick={() => { uploadedFiles = []; showAIFeatures = false; }}>
									Clear all
								</button>
							</div>

							{#each uploadedFiles as file, index}
								<div class="file-item">
									<div class="file-icon-wrapper">
										<span class="file-icon-emoji">{getFileIcon(file.name)}</span>
									</div>
									<div class="file-info">
										<div class="file-name">{file.name}</div>
										<div class="file-meta">
											<span class="file-size">{formatFileSize(file.size)}</span>
											{#if isUploading}
												<span class="badge badge-primary">
													<span class="spinner"></span>
													Uploading...
												</span>
											{:else if showAIFeatures}
												<span class="badge badge-success">✓ Ready</span>
											{/if}
										</div>
									</div>
									<button 
										class="file-remove"
										onclick={() => removeFile(index)}
										aria-label="Remove file"
									>
										✕
									</button>
								</div>
							{/each}

							{#if isUploading}
								<div class="progress-section">
									<div class="progress-bar progress-bar-ai">
										<div class="progress-bar-fill" style="width: {uploadProgress}%"></div>
									</div>
									<p class="progress-text">Uploading and analyzing... {uploadProgress}%</p>
								</div>
							{/if}

							{#if showAIFeatures && !isUploading}
								<div class="ai-features">
									<div class="ai-feature">
										<span class="ai-icon">🛡️</span>
										<div>
											<div class="ai-feature-title">Security Scan Complete</div>
											<div class="ai-feature-text">No threats detected</div>
										</div>
									</div>
									<div class="ai-feature">
										<span class="ai-icon">🔍</span>
										<div>
											<div class="ai-feature-title">Content Analysis</div>
											<div class="ai-feature-text">Preview available for recipients</div>
										</div>
									</div>
									<div class="ai-feature">
										<span class="ai-icon">⚡</span>
										<div>
											<div class="ai-feature-title">Smart Compression</div>
											<div class="ai-feature-text">Optimized for faster transfer</div>
										</div>
									</div>
								</div>

								<div class="upload-actions">
									<button class="btn btn-ai btn-lg" style="width: 100%;">
										<span>Generate Share Link</span>
										<span>✨</span>
									</button>
								</div>
							{/if}
						</div>
					{/if}
				</div>

				<!-- Features Grid -->
				<div class="features-grid">
					<div class="feature-card">
						<div class="feature-icon">🔒</div>
						<h4 class="feature-card-title">End-to-End Encrypted</h4>
						<p class="feature-card-text">Your files are encrypted before upload</p>
					</div>
					<div class="feature-card">
						<div class="feature-icon">👁️</div>
						<h4 class="feature-card-title">Visual Preview</h4>
						<p class="feature-card-text">See thumbnails before downloading</p>
					</div>
					<div class="feature-card">
						<div class="feature-icon">🤖</div>
						<h4 class="feature-card-title">AI Analysis</h4>
						<p class="feature-card-text">Automatic content detection</p>
					</div>
					<div class="feature-card">
						<div class="feature-icon">⚡</div>
						<h4 class="feature-card-title">Lightning Fast</h4>
						<p class="feature-card-text">Optimized transfer speeds</p>
					</div>
				</div>
			</div>

			<!-- Stats -->
			<div class="stats">
				<div class="stat">
					<div class="stat-value">10M+</div>
					<div class="stat-label">Files Shared</div>
				</div>
				<div class="stat">
					<div class="stat-value">500K+</div>
					<div class="stat-label">Active Users</div>
				</div>
				<div class="stat">
					<div class="stat-value">99.9%</div>
					<div class="stat-label">Uptime</div>
				</div>
				<div class="stat">
					<div class="stat-value">256-bit</div>
					<div class="stat-label">Encryption</div>
				</div>
			</div>
		</div>
	</div>
</section>

<!-- How It Works Section -->
<section class="section" id="how-it-works">
	<div class="container">
		<div class="section-header">
			<h2 class="section-title">How Glimpse Works</h2>
			<p class="section-subtitle">Three simple steps to secure file sharing</p>
		</div>

		<div class="steps">
			<div class="step">
				<div class="step-number">1</div>
				<div class="step-content">
					<h3 class="step-title">Upload Your Files</h3>
					<p class="step-text">
						Drag and drop or select files. Our AI automatically scans and 
						prepares them for secure transfer.
					</p>
				</div>
			</div>
			<div class="step">
				<div class="step-number">2</div>
				<div class="step-content">
					<h3 class="step-title">Generate Smart Link</h3>
					<p class="step-text">
						Get a secure, password-protected link with customizable 
						expiration and download limits.
					</p>
				</div>
			</div>
			<div class="step">
				<div class="step-number">3</div>
				<div class="step-content">
					<h3 class="step-title">Share with Confidence</h3>
					<p class="step-text">
						Recipients can preview before downloading. You get real-time 
						analytics on views and downloads.
					</p>
				</div>
			</div>
		</div>
	</div>
</section>

<!-- Features Section -->
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
			<div class="feature-showcase">
				<div class="feature-showcase-visual">
					<div class="preview-card">
						<div class="preview-header">
							<span>📄</span>
							<span class="badge badge-ai">AI Analyzed</span>
						</div>
						<div class="preview-content">
							<div class="preview-lines">
								<div class="preview-line" style="width: 90%"></div>
								<div class="preview-line" style="width: 75%"></div>
								<div class="preview-line" style="width: 95%"></div>
								<div class="preview-line" style="width: 60%"></div>
							</div>
						</div>
					</div>
				</div>
				<div class="feature-showcase-content">
					<h3 class="feature-showcase-title">Smart Content Preview</h3>
					<p class="feature-showcase-text">
						AI-powered previews for documents, images, videos, and more. 
						Recipients see what they're downloading before committing bandwidth.
					</p>
					<ul class="feature-list">
						<li>✓ Auto-generated thumbnails</li>
						<li>✓ Document text extraction</li>
						<li>✓ Metadata analysis</li>
						<li>✓ Content safety scoring</li>
					</ul>
				</div>
			</div>

			<div class="feature-showcase feature-showcase-reverse">
				<div class="feature-showcase-visual">
					<div class="security-card">
						<div class="security-shields">
							<div class="security-shield">🛡️</div>
							<div class="security-shield">🔒</div>
							<div class="security-shield">✓</div>
						</div>
						<div class="security-status">
							<div class="security-label">Security Status</div>
							<div class="security-value">Protected</div>
						</div>
					</div>
				</div>
				<div class="feature-showcase-content">
					<h3 class="feature-showcase-title">Multi-Layer Security</h3>
					<p class="feature-showcase-text">
						Enterprise-grade security with automatic virus scanning, 
						end-to-end encryption, and access controls.
					</p>
					<ul class="feature-list">
						<li>✓ Real-time virus scanning</li>
						<li>✓ 256-bit AES encryption</li>
						<li>✓ Password protection</li>
						<li>✓ Expiring links</li>
					</ul>
				</div>
			</div>
		</div>
	</div>
</section>

<!-- Footer -->
<footer class="footer">
	<div class="container">
		<div class="footer-content">
			<div class="footer-brand">
				<div class="logo">
					<svg width="32" height="32" viewBox="0 0 32 32" fill="none">
						<rect width="32" height="32" rx="8" fill="url(#footer-logo-gradient)"/>
						<path d="M16 8L8 16L16 24L24 16L16 8Z" fill="white" opacity="0.9"/>
						<circle cx="16" cy="16" r="3" fill="white"/>
						<defs>
							<linearGradient id="footer-logo-gradient" x1="0" y1="0" x2="32" y2="32">
								<stop stop-color="#8b5cf6"/>
								<stop offset="1" stop-color="#6366f1"/>
							</linearGradient>
						</defs>
					</svg>
					<span class="logo-text">Glimpse</span>
				</div>
				<p class="footer-tagline">See before you download</p>
			</div>

			<div class="footer-links">
				<div class="footer-column">
					<h4 class="footer-heading">Product</h4>
					<a href="#features" class="footer-link">Features</a>
					<a href="#pricing" class="footer-link">Pricing</a>
					<a href="#security" class="footer-link">Security</a>
					<a href="#api" class="footer-link">API</a>
				</div>
				<div class="footer-column">
					<h4 class="footer-heading">Company</h4>
					<a href="#about" class="footer-link">About</a>
					<a href="#blog" class="footer-link">Blog</a>
					<a href="#careers" class="footer-link">Careers</a>
					<a href="#contact" class="footer-link">Contact</a>
				</div>
				<div class="footer-column">
					<h4 class="footer-heading">Legal</h4>
					<a href="#privacy" class="footer-link">Privacy</a>
					<a href="#terms" class="footer-link">Terms</a>
					<a href="#cookies" class="footer-link">Cookies</a>
				</div>
			</div>
		</div>

		<div class="footer-bottom">
			<p class="footer-copyright">© 2026 Glimpse. All rights reserved.</p>
			<div class="footer-social">
				<a href="#twitter" class="social-link" aria-label="Twitter">𝕏</a>
				<a href="#github" class="social-link" aria-label="GitHub">⚡</a>
				<a href="#discord" class="social-link" aria-label="Discord">💬</a>
			</div>
		</div>
	</div>
</footer>

<style>
	/* === NAVIGATION === */
	.nav {
		position: sticky;
		top: 0;
		background: rgba(255, 255, 255, 0.8);
		backdrop-filter: blur(10px);
		border-bottom: 1px solid var(--gray-200);
		z-index: 1000;
		padding: var(--space-4) 0;
	}

	.nav-content {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.nav-brand {
		display: flex;
		align-items: center;
		gap: var(--space-3);
	}

	.logo {
		display: flex;
		align-items: center;
		gap: var(--space-2);
		text-decoration: none;
	}

	.logo-text {
		font-size: var(--text-xl);
		font-weight: var(--font-bold);
		color: var(--gray-900);
	}

	.nav-links {
		display: flex;
		align-items: center;
		gap: var(--space-6);
	}

	.nav-link {
		font-size: var(--text-sm);
		font-weight: var(--font-medium);
		color: var(--gray-600);
		transition: color var(--transition-fast);
	}

	.nav-link:hover {
		color: var(--gray-900);
	}

	/* === HERO SECTION === */
	.hero {
		background: var(--bg-gradient-hero);
		padding: var(--space-16) 0 var(--space-24);
		position: relative;
		overflow: hidden;
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

	/* === UPLOAD SECTION === */
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

	/* === FILES LIST === */
	.files-list {
		width: 100%;
	}

	.files-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: var(--space-6);
		padding-bottom: var(--space-4);
		border-bottom: 1px solid var(--gray-200);
	}

	.files-title {
		font-size: var(--text-xl);
		font-weight: var(--font-bold);
		color: var(--gray-900);
	}

	.file-item {
		display: flex;
		align-items: center;
		gap: var(--space-4);
		padding: var(--space-4);
		background: var(--gray-50);
		border-radius: var(--radius-lg);
		margin-bottom: var(--space-3);
		transition: all var(--transition-fast);
	}

	.file-item:hover {
		background: var(--gray-100);
	}

	.file-icon-wrapper {
		flex-shrink: 0;
		width: 48px;
		height: 48px;
		background: white;
		border-radius: var(--radius-lg);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: var(--text-2xl);
	}

	.file-info {
		flex: 1;
		min-width: 0;
	}

	.file-name {
		font-weight: var(--font-medium);
		color: var(--gray-900);
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		margin-bottom: var(--space-1);
	}

	.file-meta {
		display: flex;
		align-items: center;
		gap: var(--space-3);
		font-size: var(--text-sm);
	}

	.file-size {
		color: var(--gray-500);
	}

	.file-remove {
		flex-shrink: 0;
		width: 32px;
		height: 32px;
		border: none;
		background: white;
		color: var(--gray-400);
		border-radius: var(--radius-md);
		cursor: pointer;
		transition: all var(--transition-fast);
		font-size: var(--text-lg);
	}

	.file-remove:hover {
		background: var(--error);
		color: white;
	}

	/* === PROGRESS === */
	.progress-section {
		margin-top: var(--space-6);
		padding-top: var(--space-6);
		border-top: 1px solid var(--gray-200);
	}

	.progress-text {
		margin-top: var(--space-3);
		text-align: center;
		font-size: var(--text-sm);
		color: var(--gray-600);
		font-weight: var(--font-medium);
	}

	/* === AI FEATURES === */
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

	/* === FEATURES GRID === */
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
	}

	.feature-icon {
		font-size: var(--text-4xl);
		margin-bottom: var(--space-3);
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

	/* === STATS === */
	.stats {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
		gap: var(--space-8);
		margin-top: var(--space-16);
		padding-top: var(--space-12);
		border-top: 1px solid var(--gray-200);
	}

	.stat {
		text-align: center;
	}

	.stat-value {
		font-size: var(--text-3xl);
		font-weight: var(--font-bold);
		color: var(--primary-500);
		margin-bottom: var(--space-2);
	}

	.stat-label {
		font-size: var(--text-sm);
		color: var(--gray-600);
		font-weight: var(--font-medium);
	}

	/* === SECTIONS === */
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
	}

	.section-subtitle {
		font-size: var(--text-lg);
		color: var(--gray-600);
		margin: 0;
	}

	/* === STEPS === */
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
	}

	.step-text {
		color: var(--gray-600);
		margin: 0;
	}

	/* === FEATURES SHOWCASE === */
	.features-showcase {
		display: flex;
		flex-direction: column;
		gap: var(--space-20);
	}

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

	.preview-card {
		background: white;
		border-radius: var(--radius-xl);
		padding: var(--space-6);
		box-shadow: var(--shadow-xl);
	}

	.preview-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: var(--space-6);
		font-size: var(--text-2xl);
	}

	.preview-lines {
		display: flex;
		flex-direction: column;
		gap: var(--space-3);
	}

	.preview-line {
		height: 12px;
		background: var(--gray-200);
		border-radius: var(--radius-md);
		animation: pulse 2s ease-in-out infinite;
	}

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

	.feature-showcase-title {
		margin-bottom: var(--space-4);
	}

	.feature-showcase-text {
		color: var(--gray-600);
		margin-bottom: var(--space-6);
		font-size: var(--text-lg);
	}

	.feature-list {
		list-style: none;
		display: flex;
		flex-direction: column;
		gap: var(--space-3);
	}

	.feature-list li {
		color: var(--gray-700);
		font-size: var(--text-base);
	}

	/* === FOOTER === */
	.footer {
		background: var(--gray-900);
		color: var(--gray-400);
		padding: var(--space-16) 0 var(--space-8);
	}

	.footer-content {
		display: grid;
		grid-template-columns: 2fr 3fr;
		gap: var(--space-12);
		margin-bottom: var(--space-12);
		padding-bottom: var(--space-12);
		border-bottom: 1px solid var(--gray-800);
	}

	.footer-brand .logo-text {
		color: white;
	}

	.footer-tagline {
		margin-top: var(--space-3);
		color: var(--gray-500);
		font-size: var(--text-sm);
	}

	.footer-links {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: var(--space-8);
	}

	.footer-heading {
		color: white;
		font-size: var(--text-sm);
		font-weight: var(--font-semibold);
		margin-bottom: var(--space-4);
	}

	.footer-column {
		display: flex;
		flex-direction: column;
		gap: var(--space-3);
	}

	.footer-link {
		color: var(--gray-400);
		font-size: var(--text-sm);
		transition: color var(--transition-fast);
	}

	.footer-link:hover {
		color: white;
	}

	.footer-bottom {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.footer-copyright {
		font-size: var(--text-sm);
		margin: 0;
	}

	.footer-social {
		display: flex;
		gap: var(--space-4);
	}

	.social-link {
		width: 36px;
		height: 36px;
		background: var(--gray-800);
		border-radius: var(--radius-lg);
		display: flex;
		align-items: center;
		justify-content: center;
		color: var(--gray-400);
		transition: all var(--transition-fast);
	}

	.social-link:hover {
		background: var(--primary-500);
		color: white;
		transform: translateY(-2px);
	}

	/* === RESPONSIVE === */
	@media (max-width: 768px) {
		.hero {
			padding: var(--space-12) 0 var(--space-16);
		}

		.hero-title {
			font-size: var(--text-3xl);
		}

		.hero-subtitle {
			font-size: var(--text-base);
		}

		.upload-zone {
			padding: var(--space-6);
		}

		.features-grid {
			grid-template-columns: repeat(2, 1fr);
		}

		.stats {
			grid-template-columns: repeat(2, 1fr);
			gap: var(--space-6);
		}

		.feature-showcase {
			grid-template-columns: 1fr;
			gap: var(--space-8);
		}

		.feature-showcase-reverse {
			direction: ltr;
		}

		.footer-content {
			grid-template-columns: 1fr;
		}

		.footer-links {
			grid-template-columns: 1fr;
		}

		.footer-bottom {
			flex-direction: column;
			gap: var(--space-4);
			text-align: center;
		}
	}
</style>

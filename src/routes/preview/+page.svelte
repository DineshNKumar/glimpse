<script lang="ts">
	import { onMount } from 'svelte';

	// Mock file data
	let fileData = $state({
		id: 'abc123xyz',
		name: 'Project_Proposal_2026.pdf',
		size: 2458624,
		type: 'application/pdf',
		uploadedAt: '2026-02-08T14:30:00Z',
		expiresAt: '2026-02-15T14:30:00Z',
		downloads: 3,
		maxDownloads: 10,
		hasPassword: true,
		aiAnalysis: {
			contentType: 'Document',
			safetyScore: 98,
			pages: 15,
			language: 'English',
			summary: 'Business proposal document containing project overview, timeline, and budget'
		}
	});

	let isPasswordRequired = $state(true);
	let passwordInput = $state('');
	let isDownloading = $state(false);
	let downloadProgress = $state(0);
	let showPreview = $state(false);
	let isPasswordWrong = $state(false);

	function formatFileSize(bytes: number): string {
		if (bytes === 0) return '0 Bytes';
		const k = 1024;
		const sizes = ['Bytes', 'KB', 'MB', 'GB'];
		const i = Math.floor(Math.log(bytes) / Math.log(k));
		return Math.round(bytes / Math.pow(k, i) * 100) / 100 + ' ' + sizes[i];
	}

	function formatDate(dateString: string): string {
		const date = new Date(dateString);
		return date.toLocaleDateString('en-US', { 
			year: 'numeric', 
			month: 'short', 
			day: 'numeric',
			hour: '2-digit',
			minute: '2-digit'
		});
	}

	function getTimeRemaining(expiresAt: string): string {
		const now = new Date();
		const expiry = new Date(expiresAt);
		const diff = expiry.getTime() - now.getTime();
		const days = Math.floor(diff / (1000 * 60 * 60 * 24));
		const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
		
		if (days > 0) return `${days} day${days > 1 ? 's' : ''} remaining`;
		if (hours > 0) return `${hours} hour${hours > 1 ? 's' : ''} remaining`;
		return 'Expires soon';
	}

	function handlePasswordSubmit() {
		// Mock password validation
		if (passwordInput === 'demo' || passwordInput.length > 0) {
			isPasswordRequired = false;
			showPreview = true;
			isPasswordWrong = false;
		} else {
			isPasswordWrong = true;
		}
	}

	function handleDownload() {
		isDownloading = true;
		downloadProgress = 0;

		const interval = setInterval(() => {
			downloadProgress += 10;
			if (downloadProgress >= 100) {
				clearInterval(interval);
				setTimeout(() => {
					isDownloading = false;
					fileData.downloads += 1;
				}, 500);
			}
		}, 200);
	}

	onMount(() => {
		// In real app, fetch file metadata from URL params
		const urlParams = new URLSearchParams(window.location.search);
		const noPassword = urlParams.get('demo') === 'true';
		if (noPassword) {
			isPasswordRequired = false;
			showPreview = true;
		}
	});
</script>

<div class="preview-page">
	<!-- Header -->
	<header class="preview-header">
		<div class="container">
			<div class="preview-header-content">
				<a href="/" class="back-link">
					<svg width="20" height="20" viewBox="0 0 20 20" fill="none">
						<path d="M12.5 15L7.5 10L12.5 5" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
					</svg>
					Back to Home
				</a>
				<div class="logo">
					<svg width="32" height="32" viewBox="0 0 32 32" fill="none">
						<rect width="32" height="32" rx="8" fill="url(#header-gradient)"/>
						<path d="M16 8L8 16L16 24L24 16L16 8Z" fill="white" opacity="0.9"/>
						<circle cx="16" cy="16" r="3" fill="white"/>
						<defs>
							<linearGradient id="header-gradient" x1="0" y1="0" x2="32" y2="32">
								<stop stop-color="#8b5cf6"/>
								<stop offset="1" stop-color="#6366f1"/>
							</linearGradient>
						</defs>
					</svg>
					<span class="logo-text">Glimpse</span>
				</div>
			</div>
		</div>
	</header>

	<main class="preview-main">
		<div class="container">
			{#if isPasswordRequired}
				<!-- Password Protection -->
				<div class="password-screen animate-fade-in">
					<div class="password-card">
						<div class="password-icon">🔒</div>
						<h1 class="password-title">Password Protected</h1>
						<p class="password-subtitle">
							This file is password protected. Enter the password to view and download.
						</p>

						<form class="password-form" onsubmit={(e) => { e.preventDefault(); handlePasswordSubmit(); }}>
							<div class="form-group">
								<label for="password" class="form-label">Password</label>
								<input
									id="password"
									type="password"
									class="input {isPasswordWrong ? 'input-error' : ''}"
									placeholder="Enter password"
									bind:value={passwordInput}
								/>
								{#if isPasswordWrong}
									<p class="form-error">Incorrect password. Please try again.</p>
								{/if}
							</div>

							<button type="submit" class="btn btn-primary btn-lg" style="width: 100%;">
								Unlock File
							</button>
						</form>

						<div class="password-hint">
							<p class="hint-text">💡 Hint: Try "demo" for this preview</p>
						</div>
					</div>

					<div class="file-preview-minimal">
						<div class="file-icon-large">📄</div>
						<h3 class="file-name">{fileData.name}</h3>
						<p class="file-size">{formatFileSize(fileData.size)}</p>
					</div>
				</div>
			{:else}
				<!-- File Preview -->
				<div class="file-preview-screen animate-fade-in">
					<!-- Main File Card -->
					<div class="file-card-main">
						<!-- File Header -->
						<div class="file-header">
							<div class="file-icon-wrapper-large">
								<span class="file-icon-large">📄</span>
							</div>
							<div class="file-header-info">
								<h1 class="file-title">{fileData.name}</h1>
								<div class="file-meta-row">
									<span class="meta-item">
										<svg width="16" height="16" viewBox="0 0 16 16" fill="none">
											<path d="M14 6V12C14 13.1046 13.1046 14 12 14H4C2.89543 14 2 13.1046 2 12V6M14 6L8 10L2 6M14 6L8.5 2.5M2 6L8.5 2.5M8.5 2.5L8 2" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
										</svg>
										{formatFileSize(fileData.size)}
									</span>
									<span class="meta-item">
										<svg width="16" height="16" viewBox="0 0 16 16" fill="none">
											<circle cx="8" cy="8" r="6" stroke="currentColor" stroke-width="1.5"/>
											<path d="M8 4V8L10.5 9.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
										</svg>
										{formatDate(fileData.uploadedAt)}
									</span>
									<span class="meta-item">
										<svg width="16" height="16" viewBox="0 0 16 16" fill="none">
											<path d="M8 2V8M8 8L4 6M8 8L12 6M8 8V14M4 10L2 11V5L4 6M12 10L14 11V5L12 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
										</svg>
										{fileData.downloads} / {fileData.maxDownloads} downloads
									</span>
								</div>
							</div>
						</div>

						<!-- AI Analysis Section -->
						<div class="ai-analysis-section">
							<div class="section-header-small">
								<h3 class="section-title-small">
									<span class="ai-badge-inline">✨ AI</span>
									Content Analysis
								</h3>
								<div class="safety-score">
									<div class="safety-score-value">{fileData.aiAnalysis.safetyScore}%</div>
									<div class="safety-score-label">Safe</div>
								</div>
							</div>

							<div class="analysis-grid">
								<div class="analysis-item">
									<div class="analysis-label">Content Type</div>
									<div class="analysis-value">{fileData.aiAnalysis.contentType}</div>
								</div>
								<div class="analysis-item">
									<div class="analysis-label">Pages</div>
									<div class="analysis-value">{fileData.aiAnalysis.pages}</div>
								</div>
								<div class="analysis-item">
									<div class="analysis-label">Language</div>
									<div class="analysis-value">{fileData.aiAnalysis.language}</div>
								</div>
							</div>

							<div class="analysis-summary">
								<div class="analysis-label">Summary</div>
								<p class="analysis-summary-text">{fileData.aiAnalysis.summary}</p>
							</div>
						</div>

						<!-- Preview Section -->
						<div class="preview-section">
							<h3 class="section-title-small">Document Preview</h3>
							<div class="preview-container">
								<div class="preview-placeholder">
									<div class="preview-page">
										<div class="preview-page-header">
											<div class="preview-page-title"></div>
											<div class="preview-page-subtitle"></div>
										</div>
										<div class="preview-page-content">
											<div class="preview-line" style="width: 100%;"></div>
											<div class="preview-line" style="width: 95%;"></div>
											<div class="preview-line" style="width: 98%;"></div>
											<div class="preview-line" style="width: 88%;"></div>
											<div class="preview-line" style="width: 92%;"></div>
											<div class="preview-line" style="width: 100%;"></div>
											<div class="preview-line" style="width: 85%;"></div>
											<div class="preview-line" style="width: 96%;"></div>
										</div>
									</div>
									<div class="preview-overlay">
										<div class="preview-overlay-icon">👁️</div>
										<p class="preview-overlay-text">Preview available after download</p>
									</div>
								</div>
							</div>
						</div>

						<!-- Download Section -->
						<div class="download-section">
							{#if !isDownloading}
								<button class="btn btn-primary btn-lg download-btn" onclick={handleDownload}>
									<svg width="20" height="20" viewBox="0 0 20 20" fill="none">
										<path d="M10 3V13M10 13L6 9M10 13L14 9M3 17H17" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
									</svg>
									Download File
								</button>
								<p class="download-info">
									<span class="badge badge-warning">
										⏰ {getTimeRemaining(fileData.expiresAt)}
									</span>
								</p>
							{:else}
								<div class="download-progress">
									<div class="progress-header">
										<span class="progress-label">Downloading...</span>
										<span class="progress-percentage">{downloadProgress}%</span>
									</div>
									<div class="progress-bar progress-bar-ai">
										<div class="progress-bar-fill" style="width: {downloadProgress}%"></div>
									</div>
								</div>
							{/if}
						</div>
					</div>

					<!-- Sidebar -->
					<aside class="file-sidebar">
						<!-- Security Card -->
						<div class="sidebar-card">
							<h3 class="sidebar-title">🛡️ Security</h3>
							<div class="security-checks">
								<div class="security-check">
									<span class="check-icon success">✓</span>
									<span class="check-text">Virus scan complete</span>
								</div>
								<div class="security-check">
									<span class="check-icon success">✓</span>
									<span class="check-text">Encrypted transfer</span>
								</div>
								<div class="security-check">
									<span class="check-icon success">✓</span>
									<span class="check-text">Password protected</span>
								</div>
								<div class="security-check">
									<span class="check-icon success">✓</span>
									<span class="check-text">Safe to download</span>
								</div>
							</div>
						</div>

						<!-- File Info Card -->
						<div class="sidebar-card">
							<h3 class="sidebar-title">📋 Details</h3>
							<div class="file-details">
								<div class="detail-row">
									<span class="detail-label">File ID</span>
									<span class="detail-value font-mono">{fileData.id}</span>
								</div>
								<div class="detail-row">
									<span class="detail-label">Uploaded</span>
									<span class="detail-value">{formatDate(fileData.uploadedAt)}</span>
								</div>
								<div class="detail-row">
									<span class="detail-label">Expires</span>
									<span class="detail-value">{formatDate(fileData.expiresAt)}</span>
								</div>
								<div class="detail-row">
									<span class="detail-label">Downloads</span>
									<span class="detail-value">{fileData.downloads} / {fileData.maxDownloads}</span>
								</div>
							</div>
						</div>

						<!-- Share Card -->
						<div class="sidebar-card">
							<h3 class="sidebar-title">🔗 Share</h3>
							<p class="sidebar-text">Send this link to others to let them download this file.</p>
							<div class="share-link-container">
								<input 
									type="text" 
									class="share-link-input" 
									value="https://glimpse.app/d/{fileData.id}"
									readonly
								/>
								<button class="btn btn-secondary btn-sm copy-btn" onclick={() => alert('Link copied!')}>
									Copy
								</button>
							</div>
						</div>
					</aside>
				</div>
			{/if}
		</div>
	</main>
</div>

<style>
	.preview-page {
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		background: var(--gray-50);
	}

	/* === HEADER === */
	.preview-header {
		background: white;
		border-bottom: 1px solid var(--gray-200);
		padding: var(--space-4) 0;
		position: sticky;
		top: 0;
		z-index: 100;
		backdrop-filter: blur(10px);
	}

	.preview-header-content {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.back-link {
		display: flex;
		align-items: center;
		gap: var(--space-2);
		color: var(--gray-600);
		font-size: var(--text-sm);
		font-weight: var(--font-medium);
		transition: color var(--transition-fast);
	}

	.back-link:hover {
		color: var(--gray-900);
	}

	.logo {
		display: flex;
		align-items: center;
		gap: var(--space-2);
	}

	.logo-text {
		font-size: var(--text-xl);
		font-weight: var(--font-bold);
		color: var(--gray-900);
	}

	/* === MAIN === */
	.preview-main {
		flex: 1;
		padding: var(--space-12) 0;
	}

	/* === PASSWORD SCREEN === */
	.password-screen {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: var(--space-12);
		max-width: 1000px;
		margin: 0 auto;
		align-items: center;
	}

	.password-card {
		background: white;
		border-radius: var(--radius-2xl);
		padding: var(--space-12);
		box-shadow: var(--shadow-lg);
	}

	.password-icon {
		font-size: 4rem;
		text-align: center;
		margin-bottom: var(--space-6);
	}

	.password-title {
		text-align: center;
		margin-bottom: var(--space-3);
	}

	.password-subtitle {
		text-align: center;
		color: var(--gray-600);
		margin-bottom: var(--space-8);
	}

	.password-form {
		margin-bottom: var(--space-6);
	}

	.form-group {
		margin-bottom: var(--space-6);
	}

	.form-label {
		display: block;
		font-size: var(--text-sm);
		font-weight: var(--font-medium);
		color: var(--gray-700);
		margin-bottom: var(--space-2);
	}

	.input-error {
		border-color: var(--error);
	}

	.form-error {
		margin-top: var(--space-2);
		font-size: var(--text-sm);
		color: var(--error);
	}

	.password-hint {
		padding: var(--space-4);
		background: var(--primary-50);
		border-radius: var(--radius-lg);
		text-align: center;
	}

	.hint-text {
		font-size: var(--text-sm);
		color: var(--primary-700);
		margin: 0;
	}

	.file-preview-minimal {
		text-align: center;
		background: white;
		border-radius: var(--radius-2xl);
		padding: var(--space-12);
		box-shadow: var(--shadow-lg);
	}

	.file-icon-large {
		font-size: 6rem;
		margin-bottom: var(--space-6);
	}

	.file-name {
		font-size: var(--text-2xl);
		font-weight: var(--font-bold);
		color: var(--gray-900);
		margin-bottom: var(--space-2);
	}

	.file-size {
		font-size: var(--text-lg);
		color: var(--gray-500);
		margin: 0;
	}

	/* === FILE PREVIEW SCREEN === */
	.file-preview-screen {
		display: grid;
		grid-template-columns: 1fr 350px;
		gap: var(--space-8);
		max-width: 1400px;
		margin: 0 auto;
	}

	.file-card-main {
		background: white;
		border-radius: var(--radius-2xl);
		box-shadow: var(--shadow-md);
		overflow: hidden;
	}

	/* === FILE HEADER === */
	.file-header {
		padding: var(--space-8);
		border-bottom: 1px solid var(--gray-200);
		display: flex;
		gap: var(--space-6);
		align-items: flex-start;
	}

	.file-icon-wrapper-large {
		flex-shrink: 0;
		width: 80px;
		height: 80px;
		background: var(--primary-50);
		border-radius: var(--radius-xl);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 3rem;
	}

	.file-header-info {
		flex: 1;
	}

	.file-title {
		font-size: var(--text-3xl);
		margin-bottom: var(--space-4);
		word-break: break-word;
	}

	.file-meta-row {
		display: flex;
		flex-wrap: wrap;
		gap: var(--space-6);
	}

	.meta-item {
		display: flex;
		align-items: center;
		gap: var(--space-2);
		font-size: var(--text-sm);
		color: var(--gray-600);
	}

	.meta-item svg {
		color: var(--gray-400);
	}

	/* === AI ANALYSIS === */
	.ai-analysis-section {
		padding: var(--space-8);
		background: var(--primary-50);
		border-bottom: 1px solid var(--primary-100);
	}

	.section-header-small {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: var(--space-6);
	}

	.section-title-small {
		font-size: var(--text-xl);
		font-weight: var(--font-bold);
		display: flex;
		align-items: center;
		gap: var(--space-2);
	}

	.ai-badge-inline {
		display: inline-flex;
		align-items: center;
		padding: var(--space-1) var(--space-2);
		background: var(--bg-gradient-ai);
		color: white;
		border-radius: var(--radius-md);
		font-size: var(--text-xs);
		font-weight: var(--font-semibold);
	}

	.safety-score {
		text-align: right;
	}

	.safety-score-value {
		font-size: var(--text-3xl);
		font-weight: var(--font-bold);
		color: var(--success);
		line-height: 1;
	}

	.safety-score-label {
		font-size: var(--text-sm);
		color: var(--gray-600);
	}

	.analysis-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: var(--space-4);
		margin-bottom: var(--space-6);
	}

	.analysis-item {
		background: white;
		padding: var(--space-4);
		border-radius: var(--radius-lg);
	}

	.analysis-label {
		font-size: var(--text-xs);
		font-weight: var(--font-medium);
		color: var(--gray-500);
		text-transform: uppercase;
		letter-spacing: 0.5px;
		margin-bottom: var(--space-2);
	}

	.analysis-value {
		font-size: var(--text-lg);
		font-weight: var(--font-semibold);
		color: var(--gray-900);
	}

	.analysis-summary {
		background: white;
		padding: var(--space-4);
		border-radius: var(--radius-lg);
	}

	.analysis-summary-text {
		font-size: var(--text-base);
		color: var(--gray-700);
		line-height: var(--leading-relaxed);
		margin: 0;
	}

	/* === PREVIEW SECTION === */
	.preview-section {
		padding: var(--space-8);
		border-bottom: 1px solid var(--gray-200);
	}

	.preview-container {
		margin-top: var(--space-6);
	}

	.preview-placeholder {
		position: relative;
		background: var(--gray-100);
		border-radius: var(--radius-xl);
		overflow: hidden;
		aspect-ratio: 8.5 / 11;
	}

	.preview-page {
		padding: var(--space-8);
		background: white;
		height: 100%;
	}

	.preview-page-header {
		margin-bottom: var(--space-8);
	}

	.preview-page-title {
		height: 24px;
		background: var(--gray-300);
		border-radius: var(--radius-md);
		width: 60%;
		margin-bottom: var(--space-3);
	}

	.preview-page-subtitle {
		height: 16px;
		background: var(--gray-200);
		border-radius: var(--radius-md);
		width: 40%;
	}

	.preview-page-content {
		display: flex;
		flex-direction: column;
		gap: var(--space-3);
	}

	.preview-line {
		height: 10px;
		background: var(--gray-200);
		border-radius: var(--radius-sm);
	}

	.preview-overlay {
		position: absolute;
		inset: 0;
		background: rgba(255, 255, 255, 0.95);
		backdrop-filter: blur(8px);
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: var(--space-4);
	}

	.preview-overlay-icon {
		font-size: 4rem;
	}

	.preview-overlay-text {
		font-size: var(--text-lg);
		font-weight: var(--font-medium);
		color: var(--gray-600);
		margin: 0;
	}

	/* === DOWNLOAD SECTION === */
	.download-section {
		padding: var(--space-8);
		text-align: center;
	}

	.download-btn {
		min-width: 300px;
		margin-bottom: var(--space-4);
	}

	.download-info {
		display: flex;
		justify-content: center;
		gap: var(--space-3);
		margin: 0;
	}

	.download-progress {
		max-width: 400px;
		margin: 0 auto;
	}

	.progress-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: var(--space-3);
	}

	.progress-label {
		font-weight: var(--font-medium);
		color: var(--gray-700);
	}

	.progress-percentage {
		font-weight: var(--font-bold);
		color: var(--primary-500);
	}

	/* === SIDEBAR === */
	.file-sidebar {
		display: flex;
		flex-direction: column;
		gap: var(--space-6);
	}

	.sidebar-card {
		background: white;
		border-radius: var(--radius-xl);
		padding: var(--space-6);
		box-shadow: var(--shadow-sm);
	}

	.sidebar-title {
		font-size: var(--text-lg);
		font-weight: var(--font-bold);
		margin-bottom: var(--space-4);
		display: flex;
		align-items: center;
		gap: var(--space-2);
	}

	.sidebar-text {
		font-size: var(--text-sm);
		color: var(--gray-600);
		margin-bottom: var(--space-4);
	}

	/* === SECURITY CHECKS === */
	.security-checks {
		display: flex;
		flex-direction: column;
		gap: var(--space-3);
	}

	.security-check {
		display: flex;
		align-items: center;
		gap: var(--space-3);
		font-size: var(--text-sm);
	}

	.check-icon {
		width: 20px;
		height: 20px;
		border-radius: var(--radius-full);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: var(--text-xs);
		font-weight: var(--font-bold);
		flex-shrink: 0;
	}

	.check-icon.success {
		background: var(--success);
		color: white;
	}

	.check-text {
		color: var(--gray-700);
	}

	/* === FILE DETAILS === */
	.file-details {
		display: flex;
		flex-direction: column;
		gap: var(--space-3);
	}

	.detail-row {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding-bottom: var(--space-3);
		border-bottom: 1px solid var(--gray-100);
	}

	.detail-row:last-child {
		border-bottom: none;
		padding-bottom: 0;
	}

	.detail-label {
		font-size: var(--text-sm);
		color: var(--gray-500);
	}

	.detail-value {
		font-size: var(--text-sm);
		font-weight: var(--font-medium);
		color: var(--gray-900);
	}

	/* === SHARE LINK === */
	.share-link-container {
		display: flex;
		gap: var(--space-2);
	}

	.share-link-input {
		flex: 1;
		padding: var(--space-2) var(--space-3);
		font-size: var(--text-sm);
		font-family: var(--font-mono);
		background: var(--gray-50);
		border: 1px solid var(--gray-200);
		border-radius: var(--radius-md);
		color: var(--gray-700);
	}

	.copy-btn {
		flex-shrink: 0;
	}

	/* === RESPONSIVE === */
	@media (max-width: 1024px) {
		.file-preview-screen {
			grid-template-columns: 1fr;
		}

		.file-sidebar {
			order: -1;
		}
	}

	@media (max-width: 768px) {
		.password-screen {
			grid-template-columns: 1fr;
		}

		.file-header {
			flex-direction: column;
			align-items: center;
			text-align: center;
		}

		.file-title {
			font-size: var(--text-2xl);
		}

		.file-meta-row {
			flex-direction: column;
			gap: var(--space-2);
		}

		.analysis-grid {
			grid-template-columns: 1fr;
		}

		.download-btn {
			min-width: 100%;
		}
	}
</style>

<script lang="ts">
	import { Icon, Button, Badge } from '$lib/components';
	import Logo from '$lib/components/Logo.svelte';

	let email = $state('');
	let password = $state('');
	let isLoading = $state(false);
	let error = $state('');

	async function handleLogin(e: Event) {
		e.preventDefault();
		error = '';
		isLoading = true;

		// Simulate API call
		setTimeout(() => {
			isLoading = false;
			// For demo purposes
			if (email && password) {
				// Success - redirect to home
				window.location.href = '/';
			} else {
				error = 'Please fill in all fields';
			}
		}, 1500);
	}
</script>

<svelte:head>
	<title>Sign In - Glimpse</title>
</svelte:head>

<div class="auth-page">
	<div class="auth-container">
		<!-- Left Side - Branding -->
		<div class="auth-branding">
			<div class="branding-content">
				<!-- Logo -->
				<a href="/" class="logo-link">
					<div class="logo-large">
						<Logo size={78} />
					</div>
					<h1 class="brand-name">Glimpse</h1>
				</a>

				<p class="brand-tagline">See before you download</p>

				<!-- Features -->
				<div class="features-list">
					<div class="feature-item">
						<div class="feature-icon">
							<Icon name="shield" size={24} />
						</div>
						<div class="feature-text">
							<h3>Secure & Encrypted</h3>
							<p>256-bit end-to-end encryption</p>
						</div>
					</div>
					<div class="feature-item">
						<div class="feature-icon">
							<Icon name="sparkles" size={24} />
						</div>
						<div class="feature-text">
							<h3>AI-Powered</h3>
							<p>Smart content analysis</p>
						</div>
					</div>
					<div class="feature-item">
						<div class="feature-icon">
							<Icon name="eye" size={24} />
						</div>
						<div class="feature-text">
							<h3>Visual Preview</h3>
							<p>See files before downloading</p>
						</div>
					</div>
				</div>

				<!-- Stats -->
				<div class="auth-stats">
					<h3 class="stats-title">Trusted by thousands</h3>
					<div class="stats-grid">
						<div class="stat-card">
							<div class="stat-icon">
								<Icon name="file-text" size={24} />
							</div>
							<div class="stat-value">10M+</div>
							<div class="stat-label">Files Shared</div>
						</div>
						<div class="stat-card">
							<div class="stat-icon">
								<Icon name="users" size={24} />
							</div>
							<div class="stat-value">500K+</div>
							<div class="stat-label">Active Users</div>
						</div>
						<div class="stat-card">
							<div class="stat-icon">
								<Icon name="activity" size={24} />
							</div>
							<div class="stat-value">99.9%</div>
							<div class="stat-label">Uptime</div>
						</div>
						<div class="stat-card">
							<div class="stat-icon">
								<Icon name="shield" size={24} />
							</div>
							<div class="stat-value">256-bit</div>
							<div class="stat-label">Encryption</div>
						</div>
					</div>
				</div>
			</div>
		</div>

		<!-- Right Side - Login Form -->
		<div class="auth-form-side">
			<div class="auth-form-container">
				<!-- Back to Home -->
				<a href="/" class="back-link">
					<Icon name="arrow-left" size={20} />
					<span>Back to Home</span>
				</a>

				<!-- Form Header -->
				<div class="form-header">
					<h2 class="form-title">Welcome back</h2>
					<p class="form-subtitle">Sign in to your account to continue</p>
				</div>

				<!-- Login Form -->
				<form class="auth-form" onsubmit={handleLogin}>
					{#if error}
						<div class="alert alert-error animate-fade-in">
							<Icon name="exclamation" size={20} />
							<span>{error}</span>
						</div>
					{/if}

					<div class="form-group">
						<label for="email" class="form-label">Email address</label>
						<div class="input-group">
							<Icon name="mail" />
							<input
								id="email"
								type="email"
								class="input"
								placeholder="you@example.com"
								bind:value={email}
								required
								autocomplete="email"
							/>
						</div>
					</div>

					<div class="form-group">
						<div class="form-label-row">
							<label for="password" class="form-label">Password</label>
							<a href="/forgot-password" class="forgot-link">Forgot password?</a>
						</div>
						<div class="input-group">
							<Icon name="lock" />
							<input
								id="password"
								type="password"
								class="input"
								placeholder="••••••••"
								bind:value={password}
								required
								autocomplete="current-password"
							/>
						</div>
					</div>

					<div class="form-group">
						<label class="checkbox-label">
							<input type="checkbox" class="checkbox" />
							<span>Remember me for 30 days</span>
						</label>
					</div>

					<Button variant="primary" size="lg" type="submit" loading={isLoading} class="w-full">
						{#if isLoading}
							<span>Signing in...</span>
						{:else}
							<span>Sign In</span>
							<Icon name="arrow-right" size={20} />
						{/if}
					</Button>

					<div class="divider-text">
						<span>Or continue with</span>
					</div>

					<div class="social-buttons">
						<button type="button" class="btn-social">
							<svg width="20" height="20" viewBox="0 0 20 20" fill="currentColor">
								<path
									d="M10 0C4.477 0 0 4.477 0 10c0 4.991 3.657 9.128 8.438 9.879V12.89h-2.54V10h2.54V7.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V10h2.773l-.443 2.89h-2.33v6.989C16.343 19.129 20 14.99 20 10c0-5.523-4.477-10-10-10z"
								/>
							</svg>
							<span>Facebook</span>
						</button>
						<button type="button" class="btn-social">
							<svg width="20" height="20" viewBox="0 0 20 20" fill="currentColor">
								<path
									d="M19.6 10.23c0-.82-.1-1.42-.25-2.05H10v3.72h5.5c-.15.96-.74 2.31-2.04 3.22v2.45h3.16c1.89-1.73 2.98-4.3 2.98-7.34z"
								/>
								<path
									d="M13.46 15.13c-.83.59-1.96 1-3.46 1-2.64 0-4.88-1.74-5.68-4.15H1.07v2.52C2.72 17.75 6.09 20 10 20c2.7 0 4.96-.89 6.62-2.42l-3.16-2.45z"
								/>
								<path
									d="M3.99 10c0-.69.12-1.35.32-1.97V5.51H1.07A9.973 9.973 0 000 10c0 1.61.39 3.14 1.07 4.49l3.24-2.52c-.2-.62-.32-1.28-.32-1.97z"
								/>
								<path
									d="M10 3.88c1.88 0 3.13.81 3.85 1.48l2.84-2.76C14.96.99 12.7 0 10 0 6.09 0 2.72 2.25 1.07 5.51l3.24 2.52C5.12 5.62 7.36 3.88 10 3.88z"
								/>
							</svg>
							<span>Google</span>
						</button>
					</div>
				</form>

				<!-- Sign Up Link -->
				<div class="auth-footer">
					<p class="footer-text">
						Don't have an account?
						<a href="/signup" class="footer-link">Sign up for free</a>
					</p>
				</div>
			</div>
		</div>
	</div>
</div>

<style>
	.auth-page {
		min-height: 100vh;
		background: var(--gray-50);
		display: flex;
		align-items: center;
		justify-content: center;
		padding: var(--space-4);
	}

	.auth-container {
		display: grid;
		grid-template-columns: 1fr 1fr;
		max-width: 1200px;
		width: 100%;
		background: white;
		border-radius: var(--radius-2xl);
		box-shadow: var(--shadow-2xl);
		overflow: hidden;
		min-height: 700px;
	}

	/* === BRANDING SIDE === */
	.auth-branding {
		background: var(--bg-gradient-ai);
		padding: var(--space-12);
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;
		overflow: hidden;
	}

	.auth-branding::before {
		content: '';
		position: absolute;
		top: -50%;
		right: -50%;
		width: 100%;
		height: 100%;
		background: radial-gradient(circle, rgba(255, 255, 255, 0.1) 0%, transparent 70%);
		animation: float 6s ease-in-out infinite;
	}

	.branding-content {
		position: relative;
		z-index: 1;
		color: white;
		text-align: center;
		max-width: 400px;
	}

	.logo-link {
		display: inline-flex;
		flex-direction: column;
		align-items: center;
		gap: var(--space-4);
		margin-bottom: var(--space-6);
		text-decoration: none;
	}

	.logo-large {
		animation: float 3s ease-in-out infinite;
	}

	.brand-name {
		font-size: var(--text-4xl);
		font-weight: var(--font-bold);
		color: white;
		margin: 0;
	}

	.brand-tagline {
		font-size: var(--text-lg);
		opacity: 0.9;
		margin-bottom: var(--space-12);
	}

	.features-list {
		display: flex;
		flex-direction: column;
		gap: var(--space-6);
		margin-bottom: var(--space-12);
	}

	.feature-item {
		display: flex;
		gap: var(--space-4);
		align-items: flex-start;
		text-align: left;
	}

	.feature-icon {
		flex-shrink: 0;
		width: 48px;
		height: 48px;
		background: rgba(255, 255, 255, 0.15);
		backdrop-filter: blur(10px);
		border-radius: var(--radius-xl);
		display: flex;
		align-items: center;
		justify-content: center;
		color: white;
	}

	.feature-text h3 {
		font-size: var(--text-lg);
		font-weight: var(--font-semibold);
		margin-bottom: var(--space-1);
		color: white;
	}

	.feature-text p {
		font-size: var(--text-sm);
		opacity: 0.85;
		margin: 0;
		color: white;
	}

	.auth-stats {
		margin-top: var(--space-8);
		padding-top: var(--space-8);
		border-top: 1px solid rgba(255, 255, 255, 0.1);
	}

	.stats-title {
		font-size: var(--text-lg);
		font-weight: var(--font-bold);
		color: white;
		margin-bottom: var(--space-6);
		text-align: center;
	}

	.stats-grid {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: var(--space-4);
	}

	.stat-card {
		background: rgba(255, 255, 255, 0.1);
		backdrop-filter: blur(10px);
		border: 1px solid rgba(255, 255, 255, 0.15);
		border-radius: var(--radius-xl);
		padding: var(--space-5);
		text-align: center;
		transition: all var(--transition-base);
	}

	.stat-card:hover {
		background: rgba(255, 255, 255, 0.15);
		border-color: rgba(255, 255, 255, 0.25);
		transform: translateY(-2px);
	}

	.stat-icon {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 48px;
		height: 48px;
		margin: 0 auto var(--space-3);
		background: rgba(255, 255, 255, 0.15);
		border-radius: var(--radius-xl);
		color: white;
	}

	.stat-value {
		font-size: var(--text-3xl);
		font-weight: var(--font-black);
		color: white;
		margin-bottom: var(--space-2);
		line-height: 1;
	}

	.stat-label {
		font-size: var(--text-sm);
		color: white;
		opacity: 0.85;
		font-weight: var(--font-medium);
	}

	/* === FORM SIDE === */
	.auth-form-side {
		padding: var(--space-12);
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.auth-form-container {
		width: 100%;
		max-width: 420px;
	}

	.back-link {
		display: inline-flex;
		align-items: center;
		gap: var(--space-2);
		color: var(--gray-600);
		font-size: var(--text-sm);
		font-weight: var(--font-medium);
		margin-bottom: var(--space-8);
		transition: color var(--transition-fast);
	}

	.back-link:hover {
		color: var(--gray-900);
	}

	.form-header {
		margin-bottom: var(--space-8);
	}

	.form-title {
		font-size: var(--text-3xl);
		font-weight: var(--font-bold);
		color: var(--gray-900);
		margin-bottom: var(--space-2);
	}

	.form-subtitle {
		font-size: var(--text-base);
		color: var(--gray-600);
		margin: 0;
	}

	.auth-form {
		display: flex;
		flex-direction: column;
		gap: var(--space-6);
	}

	.form-group {
		display: flex;
		flex-direction: column;
		gap: var(--space-2);
	}

	.form-label {
		font-size: var(--text-sm);
		font-weight: var(--font-medium);
		color: var(--gray-700);
	}

	.form-label-row {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.forgot-link {
		font-size: var(--text-sm);
		color: var(--primary-500);
		font-weight: var(--font-medium);
		transition: color var(--transition-fast);
	}

	.forgot-link:hover {
		color: var(--primary-600);
	}

	.checkbox-label {
		display: flex;
		align-items: center;
		gap: var(--space-2);
		font-size: var(--text-sm);
		color: var(--gray-700);
		cursor: pointer;
	}

	.checkbox {
		width: 18px;
		height: 18px;
		cursor: pointer;
		accent-color: var(--primary-500);
	}

	.alert {
		padding: var(--space-3) var(--space-4);
		border-radius: var(--radius-lg);
		display: flex;
		align-items: center;
		gap: var(--space-2);
		font-size: var(--text-sm);
	}

	.alert-error {
		background: var(--error-light);
		color: var(--error-dark);
		border: 1px solid var(--error);
	}

	.divider-text {
		position: relative;
		text-align: center;
		margin: var(--space-2) 0;
	}

	.divider-text::before {
		content: '';
		position: absolute;
		top: 50%;
		left: 0;
		right: 0;
		height: 1px;
		background: var(--gray-200);
	}

	.divider-text span {
		position: relative;
		background: white;
		padding: 0 var(--space-3);
		color: var(--gray-500);
		font-size: var(--text-sm);
	}

	.social-buttons {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: var(--space-3);
	}

	.btn-social {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: var(--space-2);
		padding: var(--space-3) var(--space-4);
		background: white;
		border: 2px solid var(--gray-200);
		border-radius: var(--radius-lg);
		font-size: var(--text-sm);
		font-weight: var(--font-medium);
		color: var(--gray-700);
		cursor: pointer;
		transition: all var(--transition-fast);
	}

	.btn-social:hover {
		border-color: var(--gray-300);
		background: var(--gray-50);
	}

	.auth-footer {
		margin-top: var(--space-8);
		padding-top: var(--space-6);
		border-top: 1px solid var(--gray-200);
		text-align: center;
	}

	.footer-text {
		font-size: var(--text-sm);
		color: var(--gray-600);
		margin: 0;
	}

	.footer-link {
		color: var(--primary-500);
		font-weight: var(--font-semibold);
		margin-left: var(--space-1);
		transition: color var(--transition-fast);
	}

	.footer-link:hover {
		color: var(--primary-600);
	}

	/* === RESPONSIVE === */
	@media (max-width: 1024px) {
		.auth-container {
			grid-template-columns: 1fr;
			max-width: 500px;
		}

		.auth-branding {
			display: none;
		}
	}

	@media (max-width: 640px) {
		.auth-page {
			padding: 0;
		}

		.auth-container {
			border-radius: 0;
			min-height: 100vh;
		}

		.auth-form-side {
			padding: var(--space-6);
		}

		.social-buttons {
			grid-template-columns: 1fr;
		}
	}
</style>

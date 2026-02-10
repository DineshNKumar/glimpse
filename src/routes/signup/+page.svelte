<script lang="ts">
	import { Icon, Button, Badge } from '$lib/components';
	import Logo from '$lib/components/Logo.svelte';

	let fullName = $state('');
	let email = $state('');
	let password = $state('');
	let confirmPassword = $state('');
	let agreeToTerms = $state(false);
	let isLoading = $state(false);
	let errors = $state<Record<string, string>>({});

	function validateForm(): boolean {
		const newErrors: Record<string, string> = {};

		if (!fullName.trim()) {
			newErrors.fullName = 'Full name is required';
		}

		if (!email.trim()) {
			newErrors.email = 'Email is required';
		} else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
			newErrors.email = 'Please enter a valid email';
		}

		if (!password) {
			newErrors.password = 'Password is required';
		} else if (password.length < 8) {
			newErrors.password = 'Password must be at least 8 characters';
		}

		if (password !== confirmPassword) {
			newErrors.confirmPassword = 'Passwords do not match';
		}

		if (!agreeToTerms) {
			newErrors.terms = 'You must agree to the terms';
		}

		errors = newErrors;
		return Object.keys(newErrors).length === 0;
	}

	async function handleSignup(e: Event) {
		e.preventDefault();

		if (!validateForm()) {
			return;
		}

		isLoading = true;

		// Simulate API call
		setTimeout(() => {
			isLoading = false;
			// Success - redirect to dashboard or home
			window.location.href = '/';
		}, 2000);
	}

	// Password strength indicator
	const passwordStrength = $derived(() => {
		if (!password) return { label: '', strength: 0, color: '' };

		let strength = 0;
		if (password.length >= 8) strength++;
		if (password.length >= 12) strength++;
		if (/[a-z]/.test(password) && /[A-Z]/.test(password)) strength++;
		if (/\d/.test(password)) strength++;
		if (/[^a-zA-Z0-9]/.test(password)) strength++;

		if (strength <= 2)
			return { label: 'Weak', strength: (strength / 5) * 100, color: 'var(--error)' };
		if (strength <= 3)
			return { label: 'Fair', strength: (strength / 5) * 100, color: 'var(--warning)' };
		if (strength <= 4)
			return { label: 'Good', strength: (strength / 5) * 100, color: 'var(--info)' };
		return { label: 'Strong', strength: 100, color: 'var(--success)' };
	});
</script>

<svelte:head>
	<title>Sign Up - Glimpse</title>
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

				<!-- Benefits -->
				<div class="benefits-list">
					<h3 class="benefits-title">Why choose Glimpse?</h3>
					<div class="benefit-item">
						<Icon name="check-circle" size={20} />
						<span>Free forever for personal use</span>
					</div>
					<div class="benefit-item">
						<Icon name="check-circle" size={20} />
						<span>Upload files up to 5GB</span>
					</div>
					<div class="benefit-item">
						<Icon name="check-circle" size={20} />
						<span>AI-powered content analysis</span>
					</div>
					<div class="benefit-item">
						<Icon name="check-circle" size={20} />
						<span>Military-grade encryption</span>
					</div>
					<div class="benefit-item">
						<Icon name="check-circle" size={20} />
						<span>Preview before download</span>
					</div>
					<div class="benefit-item">
						<Icon name="check-circle" size={20} />
						<span>Real-time analytics</span>
					</div>
				</div>

				<!-- Stats Section -->
				<div class="stats-section">
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

				<!-- Testimonial -->
				<div class="testimonial">
					<p class="testimonial-text">
						"Glimpse has transformed how we share files with clients. The preview feature is a
						game-changer!"
					</p>
					<div class="testimonial-author">
						<div class="author-avatar">JD</div>
						<div>
							<div class="author-name">Jane Doe</div>
							<div class="author-title">Product Designer</div>
						</div>
					</div>
				</div>
			</div>
		</div>

		<!-- Right Side - Signup Form -->
		<div class="auth-form-side">
			<div class="auth-form-container">
				<!-- Back to Home -->
				<a href="/" class="back-link">
					<Icon name="arrow-left" size={20} />
					<span>Back to Home</span>
				</a>

				<!-- Form Header -->
				<div class="form-header">
					<h2 class="form-title">Create your account</h2>
					<p class="form-subtitle">Start sharing files securely in minutes</p>
				</div>

				<!-- Signup Form -->
				<form class="auth-form" onsubmit={handleSignup}>
					<div class="form-group">
						<label for="fullName" class="form-label">Full name</label>
						<div class="input-group">
							<Icon name="user" />
							<input
								id="fullName"
								type="text"
								class="input {errors.fullName ? 'input-error' : ''}"
								placeholder="John Doe"
								bind:value={fullName}
								autocomplete="name"
							/>
						</div>
						{#if errors.fullName}
							<span class="error-text">{errors.fullName}</span>
						{/if}
					</div>

					<div class="form-group">
						<label for="email" class="form-label">Email address</label>
						<div class="input-group">
							<Icon name="mail" />
							<input
								id="email"
								type="email"
								class="input {errors.email ? 'input-error' : ''}"
								placeholder="you@example.com"
								bind:value={email}
								autocomplete="email"
							/>
						</div>
						{#if errors.email}
							<span class="error-text">{errors.email}</span>
						{/if}
					</div>

					<div class="form-group">
						<label for="password" class="form-label">Password</label>
						<div class="input-group">
							<Icon name="lock" />
							<input
								id="password"
								type="password"
								class="input {errors.password ? 'input-error' : ''}"
								placeholder="••••••••"
								bind:value={password}
								autocomplete="new-password"
							/>
						</div>
						{#if password}
							<div class="password-strength">
								<div class="strength-bar">
									<div
										class="strength-fill"
										style="width: {passwordStrength().strength}%; background: {passwordStrength().color}"
									></div>
								</div>
								<span class="strength-label" style="color: {passwordStrength().color}">
									{passwordStrength().label}
								</span>
							</div>
						{/if}
						{#if errors.password}
							<span class="error-text">{errors.password}</span>
						{/if}
					</div>

					<div class="form-group">
						<label for="confirmPassword" class="form-label">Confirm password</label>
						<div class="input-group">
							<Icon name="lock" />
							<input
								id="confirmPassword"
								type="password"
								class="input {errors.confirmPassword ? 'input-error' : ''}"
								placeholder="••••••••"
								bind:value={confirmPassword}
								autocomplete="new-password"
							/>
						</div>
						{#if errors.confirmPassword}
							<span class="error-text">{errors.confirmPassword}</span>
						{/if}
					</div>

					<div class="form-group">
						<label class="checkbox-label">
							<input type="checkbox" class="checkbox" bind:checked={agreeToTerms} />
							<span>
								I agree to the
								<a href="/terms" class="inline-link">Terms of Service</a>
								and
								<a href="/privacy" class="inline-link">Privacy Policy</a>
							</span>
						</label>
						{#if errors.terms}
							<span class="error-text">{errors.terms}</span>
						{/if}
					</div>

					<Button variant="gradient" size="lg" type="submit" loading={isLoading} class="w-full">
						{#if isLoading}
							<span>Creating account...</span>
						{:else}
							<span>Create Account</span>
							<Icon name="arrow-right" size={20} />
						{/if}
					</Button>

					<div class="divider-text">
						<span>Or sign up with</span>
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
						<button type="button" class="btn-social">
							<svg width="20" height="20" viewBox="0 0 20 20" fill="currentColor">
								<path
									d="M10 0C4.477 0 0 4.484 0 10.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0110 4.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.203 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.942.359.31.678.921.678 1.856 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0020 10.017C20 4.484 15.522 0 10 0z"
								/>
							</svg>
							<span>GitHub</span>
						</button>
					</div>
				</form>

				<!-- Login Link -->
				<div class="auth-footer">
					<p class="footer-text">
						Already have an account?
						<a href="/login" class="footer-link">Sign in</a>
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
		min-height: 750px;
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
		margin-bottom: var(--space-8);
	}

	.benefits-list {
		text-align: left;
		margin-bottom: var(--space-8);
	}

	.benefits-title {
		font-size: var(--text-xl);
		font-weight: var(--font-bold);
		color: white;
		margin-bottom: var(--space-4);
	}

	.benefit-item {
		display: flex;
		align-items: center;
		gap: var(--space-3);
		padding: var(--space-2) 0;
		font-size: var(--text-sm);
		color: white;
		opacity: 0.95;
	}

	/* === STATS SECTION === */
	.stats-section {
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

	.testimonial {
		background: rgba(255, 255, 255, 0.1);
		backdrop-filter: blur(10px);
		border-radius: var(--radius-xl);
		padding: var(--space-6);
		margin-top: var(--space-8);
	}

	.testimonial-text {
		font-size: var(--text-sm);
		font-style: italic;
		margin-bottom: var(--space-4);
		color: white;
		opacity: 0.95;
		line-height: var(--leading-relaxed);
	}

	.testimonial-author {
		display: flex;
		align-items: center;
		gap: var(--space-3);
	}

	.author-avatar {
		width: 40px;
		height: 40px;
		border-radius: var(--radius-full);
		background: rgba(255, 255, 255, 0.2);
		display: flex;
		align-items: center;
		justify-content: center;
		font-weight: var(--font-bold);
		color: white;
	}

	.author-name {
		font-size: var(--text-sm);
		font-weight: var(--font-semibold);
		color: white;
	}

	.author-title {
		font-size: var(--text-xs);
		color: white;
		opacity: 0.85;
	}

	/* === FORM SIDE === */
	.auth-form-side {
		padding: var(--space-12);
		display: flex;
		align-items: center;
		justify-content: center;
		overflow-y: auto;
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
		gap: var(--space-5);
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

	.error-text {
		font-size: var(--text-xs);
		color: var(--error);
		margin-top: var(--space-1);
	}

	.checkbox-label {
		display: flex;
		align-items: flex-start;
		gap: var(--space-2);
		font-size: var(--text-sm);
		color: var(--gray-700);
		cursor: pointer;
		line-height: var(--leading-snug);
	}

	.checkbox {
		width: 18px;
		height: 18px;
		cursor: pointer;
		accent-color: var(--primary-500);
		margin-top: 2px;
		flex-shrink: 0;
	}

	.inline-link {
		color: var(--primary-500);
		font-weight: var(--font-medium);
		transition: color var(--transition-fast);
	}

	.inline-link:hover {
		color: var(--primary-600);
		text-decoration: underline;
	}

	.password-strength {
		display: flex;
		align-items: center;
		gap: var(--space-2);
	}

	.strength-bar {
		flex: 1;
		height: 4px;
		background: var(--gray-200);
		border-radius: var(--radius-full);
		overflow: hidden;
	}

	.strength-fill {
		height: 100%;
		transition: all var(--transition-base);
		border-radius: var(--radius-full);
	}

	.strength-label {
		font-size: var(--text-xs);
		font-weight: var(--font-semibold);
		min-width: 50px;
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
		grid-template-columns: repeat(3, 1fr);
		gap: var(--space-3);
	}

	.btn-social {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: var(--space-2);
		padding: var(--space-3);
		background: white;
		border: 2px solid var(--gray-200);
		border-radius: var(--radius-lg);
		font-size: var(--text-xs);
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

		.social-buttons {
			grid-template-columns: 1fr;
		}

		.btn-social {
			flex-direction: row;
			font-size: var(--text-sm);
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
	}
</style>

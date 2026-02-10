<script lang="ts">
	import { Icon, Logo } from '$lib/components';
</script>

<footer class="footer">
	<div class="container">
		<div class="footer-content">
			<div class="footer-brand">
				<div class="footer-logo">
					<Logo size={32} />
					<span class="logo-text">Glimpse</span>
				</div>
				<p class="footer-tagline">See before you download</p>
			</div>

			<div class="footer-links">
				<div class="footer-column">
					<h4 class="footer-heading">Product</h4>
					<a href="/#features" class="footer-link">Features</a>
					<a href="/#pricing" class="footer-link">Pricing</a>
					<a href="/#security" class="footer-link">Security</a>
					<a href="/api" class="footer-link">API</a>
				</div>
				<div class="footer-column">
					<h4 class="footer-heading">Company</h4>
					<a href="/about" class="footer-link">About</a>
					<a href="/blog" class="footer-link">Blog</a>
					<a href="/careers" class="footer-link">Careers</a>
					<a href="/contact" class="footer-link">Contact</a>
				</div>
				<div class="footer-column">
					<h4 class="footer-heading">Legal</h4>
					<a href="/privacy" class="footer-link">Privacy</a>
					<a href="/terms" class="footer-link">Terms</a>
					<a href="/cookies" class="footer-link">Cookies</a>
				</div>
			</div>
		</div>

		<div class="footer-bottom">
			<p class="footer-copyright">© 2026 Glimpse. All rights reserved.</p>
			<div class="footer-social">
				<a href="#twitter" class="social-link" aria-label="Twitter">
					<Icon name="at-sign" size={20} />
				</a>
				<a href="#github" class="social-link" aria-label="GitHub">
					<Icon name="git-branch" size={20} />
				</a>
				<a href="#discord" class="social-link" aria-label="Discord">
					<Icon name="message-circle" size={20} />
				</a>
			</div>
		</div>
	</div>
</footer>

<style>
	.footer {
		background: var(--gray-900);
		color: white;
		padding: var(--space-16) 0 var(--space-8);
	}

	.footer-content {
		display: grid;
		grid-template-columns: 2fr 3fr;
		gap: var(--space-12);
		margin-bottom: var(--space-12);
	}

	.footer-brand {
		max-width: 300px;
	}

	.footer-logo {
		display: flex;
		align-items: center;
		gap: var(--space-2);
		margin-bottom: var(--space-3);
	}

	.logo-text {
		font-size: var(--text-xl);
		font-weight: var(--font-bold);
		color: white;
	}

	.footer-tagline {
		color: var(--gray-400);
		font-size: var(--text-sm);
	}

	.footer-links {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: var(--space-8);
	}

	.footer-heading {
		font-size: var(--text-sm);
		font-weight: var(--font-semibold);
		color: white;
		margin-bottom: var(--space-4);
	}

	.footer-link {
		display: block;
		font-size: var(--text-sm);
		color: var(--gray-400);
		margin-bottom: var(--space-2);
		text-decoration: none;
		transition: color var(--transition-fast);
	}

	.footer-link:hover {
		color: white;
	}

	.footer-bottom {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding-top: var(--space-8);
		border-top: 1px solid var(--gray-800);
	}

	.footer-copyright {
		font-size: var(--text-sm);
		color: var(--gray-400);
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

	@media (max-width: 768px) {
		.footer-content {
			grid-template-columns: 1fr;
			gap: var(--space-8);
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

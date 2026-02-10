<script lang="ts">
	import { Icon, Logo } from '$lib/components';
	
	interface Props {
		transparent?: boolean;
		class?: string;
	}
	
	let { transparent = false, class: className = '' }: Props = $props();
</script>

<nav class="nav {transparent ? 'nav-transparent' : ''} {className}">
	<div class="container">
		<div class="nav-content">
			<a href="/" class="nav-brand">
				<Logo size={32} />
				<span class="logo-text">Glimpse</span>
			</a>
			
			<div class="nav-links">
				<a href="/#features" class="nav-link">Features</a>
				<a href="/#how-it-works" class="nav-link hidden-mobile">How it works</a>
				<a href="/#pricing" class="nav-link">Pricing</a>
				<a href="/login" class="btn btn-secondary btn-sm">
					<Icon name="log-in" size={18} />
					<span>Login</span>
				</a>
				<a href="/signup" class="btn btn-primary btn-sm">
					<span>Sign Up</span>
				</a>
			</div>
		</div>
	</div>
</nav>

<style>
	.nav {
		position: sticky;
		top: 0;
		background: rgba(255, 255, 255, 0.95);
		backdrop-filter: blur(10px);
		border-bottom: 1px solid var(--gray-200);
		z-index: 1000;
		padding: var(--space-4) 0;
		transition: all var(--transition-base);
	}

	.nav-transparent {
		background: transparent;
		border-bottom-color: transparent;
	}

	.nav-content {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: var(--space-6);
	}

	.nav-brand {
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
		text-decoration: none;
		transition: color var(--transition-fast);
	}

	.nav-link:hover {
		color: var(--primary-600);
	}

	@media (max-width: 768px) {
		.hidden-mobile {
			display: none;
		}
		
		.nav-links {
			gap: var(--space-3);
		}
	}
</style>

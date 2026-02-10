<script lang="ts">
	import { Icon } from '$lib/components';
	import { iconNames } from '$lib/icons';

	let searchQuery = $state('');
	let copiedIcon = $state('');

	const categories = {
		'File & Upload': ['cloud-upload', 'file-text', 'file-image', 'file-video', 'file-audio', 'file-pdf', 'file-zip', 'file-code', 'files', 'folder', 'folder-open'],
		'Status & Feedback': ['check', 'check-circle', 'x-circle', 'alert-circle', 'alert-triangle', 'alert-octagon', 'info', 'info-circle', 'loader', 'refresh', 'activity'],
		'Brand Icons': ['eye (BRAND ICON)', 'sparkles (SIGNATURE ICON)', 'eye', 'sparkles'],
		'AI & Intelligence': ['brain', 'cpu', 'magic-wand', 'robot', 'zap', 'sparkles'],
		'Security': ['lock', 'unlock', 'shield', 'shield-check', 'key', 'fingerprint', 'scan'],
		'User & Profile': ['user', 'user-circle', 'users', 'user-plus', 'user-minus', 'user-check'],
		'Navigation': ['arrow-left', 'arrow-right', 'home', 'compass', 'menu', 'more-horizontal', 'more-vertical'],
		'Communication': ['mail', 'message-circle', 'message-square', 'chat', 'phone', 'send', 'bell', 'bell-off'],
		'Media & Content': ['image', 'video', 'camera', 'video-camera', 'music', 'play', 'pause', 'volume', 'mic', 'headphones'],
		'Business & Finance': ['credit-card', 'dollar-sign', 'wallet', 'receipt', 'briefcase', 'trending-up', 'trending-down', 'percent'],
		'Data & Analytics': ['bar-chart', 'pie-chart', 'line-chart', 'graph', 'database', 'server', 'activity'],
		'Developer': ['code', 'terminal', 'git-branch', 'package', 'api', 'webhook'],
		'UI Controls': ['edit', 'trash', 'copy', 'save', 'sliders', 'filter', 'sort', 'search'],
		'System': ['power', 'log-in', 'log-out', 'sun', 'moon', 'monitor', 'smartphone', 'tablet'],
		'Other': ['calendar', 'clock-history', 'globe', 'map', 'map-pin', 'tag', 'bookmark', 'star', 'heart', 'flag', 'gift', 'award']
	};

	const filteredIcons = $derived(
		searchQuery.trim() === '' 
			? iconNames 
			: iconNames.filter(name => name.toLowerCase().includes(searchQuery.toLowerCase()))
	);

	function copyIconName(name: string) {
		navigator.clipboard.writeText(`<Icon name="${name}" />`);
		copiedIcon = name;
		setTimeout(() => copiedIcon = '', 2000);
	}
</script>

<div class="icon-showcase">
	<div class="showcase-header">
		<h1>Glimpse Icon Library</h1>
		<p>{iconNames.length} professional SVG icons</p>
		
		<div class="search-box">
			<Icon name="search" size={20} />
			<input 
				type="text" 
				placeholder="Search icons..." 
				bind:value={searchQuery}
			/>
		</div>
	</div>

	{#if searchQuery.trim() === ''}
		{#each Object.entries(categories) as [category, icons]}
			<div class="category-section">
				<h2 class="category-title">{category}</h2>
				<div class="icon-grid">
					{#each icons as iconName}
						<button 
							class="icon-card {copiedIcon === iconName ? 'copied' : ''}"
							onclick={() => copyIconName(iconName)}
							title="Click to copy"
						>
							<div class="icon-preview">
								<Icon name={iconName} size={32} />
							</div>
							<div class="icon-name">{iconName}</div>
							{#if copiedIcon === iconName}
								<div class="copied-badge">Copied!</div>
							{/if}
						</button>
					{/each}
				</div>
			</div>
		{/each}
	{:else}
		<div class="category-section">
			<h2 class="category-title">Search Results ({filteredIcons.length})</h2>
			{#if filteredIcons.length > 0}
				<div class="icon-grid">
					{#each filteredIcons as iconName}
						<button 
							class="icon-card {copiedIcon === iconName ? 'copied' : ''}"
							onclick={() => copyIconName(iconName)}
							title="Click to copy"
						>
							<div class="icon-preview">
								<Icon name={iconName} size={32} />
							</div>
							<div class="icon-name">{iconName}</div>
							{#if copiedIcon === iconName}
								<div class="copied-badge">Copied!</div>
							{/if}
						</button>
					{/each}
				</div>
			{:else}
				<p class="no-results">No icons found matching "{searchQuery}"</p>
			{/if}
		</div>
	{/if}
</div>

<style>
	.icon-showcase {
		max-width: 1200px;
		margin: 0 auto;
		padding: 2rem;
	}

	.showcase-header {
		text-align: center;
		margin-bottom: 3rem;
	}

	.showcase-header h1 {
		font-size: 2.5rem;
		font-weight: 700;
		margin-bottom: 0.5rem;
		background: linear-gradient(135deg, #8b5cf6 0%, #6366f1 100%);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}

	.showcase-header p {
		color: #64748b;
		font-size: 1.125rem;
		margin-bottom: 2rem;
	}

	.search-box {
		max-width: 400px;
		margin: 0 auto;
		position: relative;
		display: flex;
		align-items: center;
		gap: 0.75rem;
		padding: 0.75rem 1rem;
		background: white;
		border: 2px solid #e2e8f0;
		border-radius: 12px;
		transition: all 0.2s;
	}

	.search-box:focus-within {
		border-color: #8b5cf6;
		box-shadow: 0 0 0 4px rgba(139, 92, 246, 0.1);
	}

	.search-box input {
		flex: 1;
		border: none;
		outline: none;
		font-size: 1rem;
	}

	.category-section {
		margin-bottom: 3rem;
	}

	.category-title {
		font-size: 1.5rem;
		font-weight: 600;
		margin-bottom: 1.5rem;
		color: #1e293b;
	}

	.icon-grid {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
		gap: 1rem;
	}

	.icon-card {
		position: relative;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.75rem;
		padding: 1.5rem 1rem;
		background: white;
		border: 2px solid #e2e8f0;
		border-radius: 12px;
		cursor: pointer;
		transition: all 0.2s;
	}

	.icon-card:hover {
		border-color: #8b5cf6;
		transform: translateY(-2px);
		box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
	}

	.icon-card.copied {
		border-color: #10b981;
		background: #f0fdf4;
	}

	.icon-preview {
		display: flex;
		align-items: center;
		justify-content: center;
		color: #64748b;
		transition: color 0.2s;
	}

	.icon-card:hover .icon-preview {
		color: #8b5cf6;
	}

	.icon-card.copied .icon-preview {
		color: #10b981;
	}

	.icon-name {
		font-size: 0.75rem;
		color: #64748b;
		text-align: center;
		word-break: break-word;
		max-width: 100%;
	}

	.copied-badge {
		position: absolute;
		top: 0.5rem;
		right: 0.5rem;
		padding: 0.25rem 0.5rem;
		background: #10b981;
		color: white;
		font-size: 0.625rem;
		font-weight: 600;
		border-radius: 6px;
		animation: fadeIn 0.2s;
	}

	.no-results {
		text-align: center;
		color: #64748b;
		padding: 2rem;
		font-size: 1.125rem;
	}

	@keyframes fadeIn {
		from {
			opacity: 0;
			transform: scale(0.8);
		}
		to {
			opacity: 1;
			transform: scale(1);
		}
	}

	@media (max-width: 768px) {
		.icon-grid {
			grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
		}

		.showcase-header h1 {
			font-size: 2rem;
		}
	}
</style>

<script lang="ts">
	import { Icon } from '$lib/components';
	
	interface Props {
		connectionType?: 'p2p' | 'server';
		fileCount?: number;
		totalSize?: number;
		estimatedCost?: number;
		onSend?: () => void;
		isSending?: boolean;
	}
	
	let {
		connectionType = 'p2p',
		fileCount = 0,
		totalSize = 0,
		estimatedCost = 0,
		onSend,
		isSending = false
	}: Props = $props();
	
	function formatFileSize(bytes: number): string {
		if (bytes === 0) return '0 Bytes';
		const k = 1024;
		const sizes = ['Bytes', 'KB', 'MB', 'GB'];
		const i = Math.floor(Math.log(bytes) / Math.log(k));
		return Math.round(bytes / Math.pow(k, i) * 100) / 100 + ' ' + sizes[i];
	}
</script>

<div class="cost-summary">
	<div class="summary-header">
		<h3 class="summary-title">Transfer Summary</h3>
	</div>
	
	<div class="summary-items">
		<!-- Connection Status -->
		<div class="summary-item">
			<div class="summary-label">
				<Icon name="wifi" size={18} />
				<span>Connection</span>
			</div>
			<div class="summary-value">
				{#if connectionType === 'p2p'}
					<span class="badge badge-success">
						<Icon name="check-circle" size={14} />
						P2P Available
					</span>
				{:else}
					<span class="badge badge-warning">
						<Icon name="server" size={14} />
						Server Upload
					</span>
				{/if}
			</div>
		</div>
		
		<!-- File Count -->
		{#if fileCount > 0}
			<div class="summary-item">
				<div class="summary-label">
					<Icon name="file" size={18} />
					<span>Files</span>
				</div>
				<div class="summary-value">
					{fileCount} {fileCount === 1 ? 'file' : 'files'}
				</div>
			</div>
		{/if}
		
		<!-- Total Size -->
		{#if totalSize > 0}
			<div class="summary-item">
				<div class="summary-label">
					<Icon name="hard-drive" size={18} />
					<span>Total Size</span>
				</div>
				<div class="summary-value">
					{formatFileSize(totalSize)}
				</div>
			</div>
		{/if}
		
		<!-- Estimated Cost -->
		<div class="summary-item highlight">
			<div class="summary-label">
				<Icon name="dollar-sign" size={18} />
				<span>Estimated Cost</span>
			</div>
			<div class="summary-value cost">
				${estimatedCost.toFixed(2)}
			</div>
		</div>
	</div>
	
	<!-- Send Button -->
	<div class="summary-action">
		<button
			type="button"
			class="btn btn-primary btn-lg btn-block"
			onclick={onSend}
			disabled={isSending || fileCount === 0}
		>
			{#if isSending}
				<Icon name="loader" size={20} />
				<span>Sending...</span>
			{:else}
				<Icon name="send" size={20} />
				<span>Send Files</span>
			{/if}
		</button>
		
		{#if connectionType === 'p2p'}
			<p class="summary-note">
				<Icon name="shield" size={14} />
				Peer-to-peer transfer • End-to-end encrypted
			</p>
		{/if}
	</div>
</div>

<style>
	.cost-summary {
		background: white;
		border: 2px solid var(--gray-200);
		border-radius: var(--radius-xl);
		padding: var(--space-6);
		margin-top: var(--space-6);
		box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
	}

	.summary-header {
		margin-bottom: var(--space-4);
		padding-bottom: var(--space-4);
		border-bottom: 1px solid var(--gray-200);
	}

	.summary-title {
		font-size: var(--text-lg);
		font-weight: var(--font-semibold);
		color: var(--gray-900);
		margin: 0;
	}

	.summary-items {
		display: flex;
		flex-direction: column;
		gap: var(--space-4);
		margin-bottom: var(--space-6);
	}

	.summary-item {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: var(--space-3);
		background: var(--gray-50);
		border-radius: var(--radius-lg);
	}

	.summary-item.highlight {
		background: var(--primary-50);
		border: 1px solid var(--primary-200);
	}

	.summary-label {
		display: flex;
		align-items: center;
		gap: var(--space-2);
		font-size: var(--text-sm);
		font-weight: var(--font-medium);
		color: var(--gray-700);
	}

	.summary-value {
		font-size: var(--text-base);
		font-weight: var(--font-semibold);
		color: var(--gray-900);
	}

	.summary-value.cost {
		font-size: var(--text-2xl);
		color: var(--primary-600);
	}

	.badge {
		display: inline-flex;
		align-items: center;
		gap: var(--space-1);
		padding: var(--space-1) var(--space-2);
		border-radius: var(--radius-md);
		font-size: var(--text-xs);
		font-weight: var(--font-semibold);
	}

	.badge-success {
		background: var(--success-light);
		color: var(--success);
	}

	.badge-warning {
		background: var(--warning-light);
		color: var(--warning);
	}

	.summary-action {
		padding-top: var(--space-4);
		border-top: 1px solid var(--gray-200);
	}

	.btn-block {
		width: 100%;
		justify-content: center;
	}

	.summary-note {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: var(--space-2);
		margin-top: var(--space-3);
		font-size: var(--text-xs);
		color: var(--gray-600);
		text-align: center;
	}

	@media (max-width: 768px) {
		.summary-item {
			flex-direction: column;
			align-items: flex-start;
			gap: var(--space-2);
		}
	}
</style>

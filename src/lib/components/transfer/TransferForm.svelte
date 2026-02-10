<script lang="ts">
	import { Icon } from '$lib/components';
	
	interface Props {
		onSubmit?: (data: TransferData) => void;
	}
	
	interface TransferData {
		recipientEmail: string;
		message: string;
		password?: string;
		expirationDays?: number;
		downloadLimit?: number;
	}
	
	let { onSubmit }: Props = $props();
	
	let recipientEmail = $state('');
	let message = $state('');
	let showAdvanced = $state(false);
	let password = $state('');
	let expirationDays = $state(7);
	let downloadLimit = $state(10);
	let enablePassword = $state(false);
	let enableExpiration = $state(true);
	let enableDownloadLimit = $state(false);
	
	function handleSubmit(e: Event) {
		e.preventDefault();
		
		const data: TransferData = {
			recipientEmail,
			message,
			...(enablePassword && password ? { password } : {}),
			...(enableExpiration ? { expirationDays } : {}),
			...(enableDownloadLimit ? { downloadLimit } : {})
		};
		
		onSubmit?.(data);
	}
</script>

<form class="transfer-form" onsubmit={handleSubmit}>
	<div class="form-section">
		<h3 class="form-section-title">Transfer Details</h3>
		
		<div class="form-group">
			<label for="recipientEmail" class="form-label">
				Recipient Email
				<span class="required">*</span>
			</label>
			<div class="input-group">
				<Icon name="mail" size={20} />
				<input
					id="recipientEmail"
					type="email"
					class="input"
					placeholder="recipient@example.com"
					bind:value={recipientEmail}
					required
				/>
			</div>
		</div>
		
		<div class="form-group">
			<label for="message" class="form-label">Message (Optional)</label>
			<textarea
				id="message"
				class="textarea"
				placeholder="Add a message for the recipient..."
				rows="4"
				bind:value={message}
			></textarea>
		</div>
	</div>
	
	<!-- Advanced Settings -->
	<div class="form-section">
		<button
			type="button"
			class="advanced-toggle"
			onclick={() => showAdvanced = !showAdvanced}
		>
			<Icon name="settings" size={20} />
			<span>Advanced Settings</span>
			<Icon name={showAdvanced ? 'chevron-up' : 'chevron-down'} size={20} />
		</button>
		
		{#if showAdvanced}
			<div class="advanced-settings animate-slide-down">
				<!-- Password Protection -->
				<div class="setting-item">
					<div class="setting-header">
						<label class="checkbox-label">
							<input
								type="checkbox"
								class="checkbox"
								bind:checked={enablePassword}
							/>
							<Icon name="lock" size={18} />
							<span>Password Protection</span>
						</label>
					</div>
					{#if enablePassword}
						<div class="setting-content">
							<input
								type="password"
								class="input"
								placeholder="Enter password"
								bind:value={password}
							/>
						</div>
					{/if}
				</div>
				
				<!-- Expiration -->
				<div class="setting-item">
					<div class="setting-header">
						<label class="checkbox-label">
							<input
								type="checkbox"
								class="checkbox"
								bind:checked={enableExpiration}
							/>
							<Icon name="calendar" size={18} />
							<span>Set Expiration</span>
						</label>
					</div>
					{#if enableExpiration}
						<div class="setting-content">
							<select class="select" bind:value={expirationDays}>
								<option value={1}>1 day</option>
								<option value={3}>3 days</option>
								<option value={7}>7 days</option>
								<option value={14}>14 days</option>
								<option value={30}>30 days</option>
							</select>
						</div>
					{/if}
				</div>
				
				<!-- Download Limit -->
				<div class="setting-item">
					<div class="setting-header">
						<label class="checkbox-label">
							<input
								type="checkbox"
								class="checkbox"
								bind:checked={enableDownloadLimit}
							/>
							<Icon name="download" size={18} />
							<span>Download Limit</span>
						</label>
					</div>
					{#if enableDownloadLimit}
						<div class="setting-content">
							<input
								type="number"
								class="input"
								min="1"
								max="100"
								bind:value={downloadLimit}
							/>
						</div>
					{/if}
				</div>
			</div>
		{/if}
	</div>
</form>

<style>
	.transfer-form {
		background: white;
		border: 1px solid var(--gray-200);
		border-radius: var(--radius-xl);
		padding: var(--space-6);
		margin-top: var(--space-6);
	}

	.form-section {
		padding: var(--space-6) 0;
		border-bottom: 1px solid var(--gray-200);
	}

	.form-section:last-child {
		border-bottom: none;
		padding-bottom: 0;
	}

	.form-section:first-child {
		padding-top: 0;
	}

	.form-section-title {
		font-size: var(--text-lg);
		font-weight: var(--font-semibold);
		color: var(--gray-900);
		margin: 0 0 var(--space-4);
	}

	.form-group {
		margin-bottom: var(--space-4);
	}

	.form-group:last-child {
		margin-bottom: 0;
	}

	.form-label {
		display: block;
		font-size: var(--text-sm);
		font-weight: var(--font-medium);
		color: var(--gray-700);
		margin-bottom: var(--space-2);
	}

	.required {
		color: var(--error);
	}

	.input-group {
		display: flex;
		align-items: center;
		gap: var(--space-3);
		padding: var(--space-3) var(--space-4);
		border: 1px solid var(--gray-300);
		border-radius: var(--radius-lg);
		background: white;
		transition: border-color var(--transition-fast);
	}

	.input-group:focus-within {
		border-color: var(--primary-500);
		outline: 2px solid var(--primary-100);
	}

	.input {
		flex: 1;
		border: none;
		outline: none;
		font-size: var(--text-base);
		color: var(--gray-900);
	}

	.input::placeholder {
		color: var(--gray-400);
	}

	.textarea {
		width: 100%;
		padding: var(--space-3);
		border: 1px solid var(--gray-300);
		border-radius: var(--radius-lg);
		font-size: var(--text-base);
		color: var(--gray-900);
		resize: vertical;
		font-family: inherit;
		transition: border-color var(--transition-fast);
	}

	.textarea:focus {
		border-color: var(--primary-500);
		outline: 2px solid var(--primary-100);
	}

	.textarea::placeholder {
		color: var(--gray-400);
	}

	.select {
		width: 100%;
		padding: var(--space-3);
		border: 1px solid var(--gray-300);
		border-radius: var(--radius-lg);
		font-size: var(--text-base);
		color: var(--gray-900);
		background: white;
		cursor: pointer;
	}

	.advanced-toggle {
		display: flex;
		align-items: center;
		gap: var(--space-2);
		padding: var(--space-3) var(--space-4);
		border: none;
		background: var(--gray-100);
		border-radius: var(--radius-lg);
		font-size: var(--text-base);
		font-weight: var(--font-medium);
		color: var(--gray-700);
		cursor: pointer;
		width: 100%;
		transition: all var(--transition-fast);
	}

	.advanced-toggle:hover {
		background: var(--gray-200);
	}

	.advanced-toggle span {
		flex: 1;
		text-align: left;
	}

	.advanced-settings {
		margin-top: var(--space-4);
		display: flex;
		flex-direction: column;
		gap: var(--space-4);
	}

	.setting-item {
		padding: var(--space-4);
		background: var(--gray-50);
		border-radius: var(--radius-lg);
	}

	.setting-header {
		margin-bottom: var(--space-3);
	}

	.checkbox-label {
		display: flex;
		align-items: center;
		gap: var(--space-2);
		font-size: var(--text-base);
		font-weight: var(--font-medium);
		color: var(--gray-700);
		cursor: pointer;
	}

	.checkbox {
		width: 18px;
		height: 18px;
		cursor: pointer;
	}

	.setting-content {
		padding-left: var(--space-8);
	}

	@keyframes slideDown {
		from {
			opacity: 0;
			transform: translateY(-10px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.animate-slide-down {
		animation: slideDown 0.3s ease-out;
	}
</style>

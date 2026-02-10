<script lang="ts">
	import { Icon } from '$lib/components';
	
	export interface FileItem {
		id: string;
		file: File;
		name?: string;
		size?: number;
		progress?: number;
		status?: 'pending' | 'uploading' | 'completed' | 'error';
	}
	
	interface Props {
		files: FileItem[];
		onRemove?: (id: string) => void;
		onClearAll?: () => void;
	}
	
	let { files, onRemove, onClearAll }: Props = $props();
	
	function formatFileSize(bytes: number): string {
		if (bytes === 0) return '0 Bytes';
		const k = 1024;
		const sizes = ['Bytes', 'KB', 'MB', 'GB'];
		const i = Math.floor(Math.log(bytes) / Math.log(k));
		return Math.round(bytes / Math.pow(k, i) * 100) / 100 + ' ' + sizes[i];
	}
	
	function getFileIcon(fileName: string): string {
		const ext = fileName.split('.').pop()?.toLowerCase();
		const iconMap: Record<string, string> = {
			'pdf': 'file-text',
			'doc': 'file-text',
			'docx': 'file-text',
			'xls': 'file-text',
			'xlsx': 'file-text',
			'txt': 'file-text',
			'jpg': 'image',
			'jpeg': 'image',
			'png': 'image',
			'gif': 'image',
			'svg': 'image',
			'mp4': 'video',
			'mov': 'video',
			'avi': 'video',
			'mp3': 'music',
			'wav': 'music',
			'zip': 'archive',
			'rar': 'archive',
			'7z': 'archive',
		};
		return iconMap[ext || ''] || 'file';
	}
</script>

{#if files.length > 0}
	<div class="file-list">
		<div class="file-list-header">
			<h3 class="file-list-title">
				{files.length} {files.length === 1 ? 'file' : 'files'} selected
			</h3>
			{#if onClearAll}
				<button 
					class="btn btn-sm btn-secondary" 
					onclick={onClearAll}
				>
					Clear all
				</button>
			{/if}
		</div>

		<div class="file-items">
			{#each files as fileItem (fileItem.id)}
				<div class="file-item">
					<div class="file-icon">
						<Icon name={getFileIcon(fileItem.file.name)} size={24} />
					</div>
					
					<div class="file-info">
						<div class="file-name">{fileItem.file.name}</div>
						<div class="file-meta">
							<span class="file-size">{formatFileSize(fileItem.file.size)}</span>
							{#if fileItem.status}
								<span class="file-status status-{fileItem.status}">
									{#if fileItem.status === 'uploading'}
										<Icon name="loader" size={14} />
										Uploading...
									{:else if fileItem.status === 'complete'}
										<Icon name="check-circle" size={14} />
										Complete
									{:else if fileItem.status === 'error'}
										<Icon name="alert-circle" size={14} />
										Error
									{/if}
								</span>
							{/if}
						</div>
						
						{#if fileItem.progress !== undefined && fileItem.status === 'uploading'}
							<div class="progress-bar">
								<div class="progress-fill" style="width: {fileItem.progress}%"></div>
							</div>
						{/if}
					</div>
					
					{#if onRemove}
						<button 
							class="file-remove"
							onclick={() => onRemove(fileItem.id)}
							aria-label="Remove file"
						>
							<Icon name="x" size={20} />
						</button>
					{/if}
				</div>
			{/each}
		</div>
	</div>
{/if}

<style>
	.file-list {
		background: white;
		border: 1px solid var(--gray-200);
		border-radius: var(--radius-xl);
		padding: var(--space-6);
		margin-top: var(--space-6);
	}

	.file-list-header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-bottom: var(--space-4);
		padding-bottom: var(--space-4);
		border-bottom: 1px solid var(--gray-200);
	}

	.file-list-title {
		font-size: var(--text-lg);
		font-weight: var(--font-semibold);
		color: var(--gray-900);
		margin: 0;
	}

	.file-items {
		display: flex;
		flex-direction: column;
		gap: var(--space-3);
	}

	.file-item {
		display: flex;
		align-items: flex-start;
		gap: var(--space-4);
		padding: var(--space-4);
		background: var(--gray-50);
		border-radius: var(--radius-lg);
		transition: background var(--transition-fast);
	}

	.file-item:hover {
		background: var(--gray-100);
	}

	.file-icon {
		flex-shrink: 0;
		width: 48px;
		height: 48px;
		display: flex;
		align-items: center;
		justify-content: center;
		background: white;
		border-radius: var(--radius-lg);
		color: var(--primary-500);
		border: 1px solid var(--gray-200);
	}

	.file-info {
		flex: 1;
		min-width: 0;
	}

	.file-name {
		font-size: var(--text-base);
		font-weight: var(--font-medium);
		color: var(--gray-900);
		margin-bottom: var(--space-1);
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.file-meta {
		display: flex;
		align-items: center;
		gap: var(--space-3);
		font-size: var(--text-sm);
		color: var(--gray-600);
	}

	.file-status {
		display: flex;
		align-items: center;
		gap: var(--space-1);
		font-weight: var(--font-medium);
	}

	.status-uploading {
		color: var(--primary-500);
	}

	.status-complete {
		color: var(--success);
	}

	.status-error {
		color: var(--error);
	}

	.progress-bar {
		width: 100%;
		height: 4px;
		background: var(--gray-200);
		border-radius: var(--radius-full);
		margin-top: var(--space-2);
		overflow: hidden;
	}

	.progress-fill {
		height: 100%;
		background: var(--primary-500);
		transition: width var(--transition-base);
	}

	.file-remove {
		flex-shrink: 0;
		width: 32px;
		height: 32px;
		display: flex;
		align-items: center;
		justify-content: center;
		border: none;
		background: transparent;
		color: var(--gray-400);
		border-radius: var(--radius-md);
		cursor: pointer;
		transition: all var(--transition-fast);
	}

	.file-remove:hover {
		background: var(--error-light);
		color: var(--error);
	}

	@media (max-width: 768px) {
		.file-item {
			flex-wrap: wrap;
		}

		.file-meta {
			flex-wrap: wrap;
		}
	}
</style>

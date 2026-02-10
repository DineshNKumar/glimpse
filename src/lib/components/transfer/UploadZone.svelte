<script lang="ts">
	import { Icon } from '$lib/components';
	
	interface Props {
		onFilesSelected?: (files: File[]) => void;
		accept?: string;
		multiple?: boolean;
		maxSize?: number; // in MB
	}
	
	let {
		onFilesSelected,
		accept = '*',
		multiple = true,
		maxSize = 5000 // 5GB default
	}: Props = $props();
	
	let isDragging = $state(false);
	let fileInput: HTMLInputElement;
	
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
		// Filter files by max size
		const validFiles = files.filter(file => {
			const fileSizeMB = file.size / (1024 * 1024);
			return fileSizeMB <= maxSize;
		});
		
		if (onFilesSelected) {
			onFilesSelected(validFiles);
		}
	}
	
	function openFileBrowser() {
		fileInput?.click();
	}
</script>

<div 
	class="upload-zone {isDragging ? 'dragging' : ''}"
	role="button"
	tabindex="0"
	ondragover={handleDragOver}
	ondragleave={handleDragLeave}
	ondrop={handleDrop}
	onclick={openFileBrowser}
	onkeydown={(e) => e.key === 'Enter' && openFileBrowser()}
>
	<div class="upload-icon">
		<Icon name="cloud-upload" size={64} />
	</div>
	
	<h3 class="upload-title">Drop files here or click to browse</h3>
	
	<p class="upload-text">
		Support for any file type • Up to {maxSize >= 1000 ? `${maxSize/1000}GB` : `${maxSize}MB`} per file
	</p>
	
	<div class="upload-options">
		<button type="button" class="btn btn-primary btn-lg" onclick={openFileBrowser}>
			<Icon name="folder-open" size={20} />
			<span>Choose Files</span>
		</button>
	</div>
	
	<div class="cloud-providers">
		<span class="cloud-label">Or upload from:</span>
		<div class="cloud-icons">
			<button type="button" class="cloud-btn" aria-label="Google Drive">
				<Icon name="box" size={20} />
			</button>
			<button type="button" class="cloud-btn" aria-label="Dropbox">
				<Icon name="package" size={20} />
			</button>
			<button type="button" class="cloud-btn" aria-label="OneDrive">
				<Icon name="cloud" size={20} />
			</button>
		</div>
	</div>
	
	<input 
		bind:this={fileInput}
		type="file" 
		{multiple}
		{accept}
		class="file-input"
		onchange={handleFileInput}
	/>
</div>

<style>
	.upload-zone {
		background: white;
		border: 3px dashed var(--gray-300);
		border-radius: var(--radius-2xl);
		padding: var(--space-12);
		text-align: center;
		cursor: pointer;
		transition: all var(--transition-base);
		min-height: 400px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: var(--space-4);
	}

	.upload-zone:hover {
		border-color: var(--primary-400);
		background: var(--primary-50);
	}

	.upload-zone.dragging {
		border-color: var(--primary-500);
		background: var(--primary-100);
		transform: scale(1.02);
	}

	.upload-icon {
		color: var(--primary-500);
		margin-bottom: var(--space-4);
	}

	.upload-title {
		font-size: var(--text-2xl);
		font-weight: var(--font-bold);
		color: var(--gray-900);
		margin: 0 0 var(--space-2);
	}

	.upload-text {
		font-size: var(--text-base);
		color: var(--gray-600);
		margin: 0 0 var(--space-8);
	}

	.upload-options {
		margin-bottom: var(--space-8);
	}

	.cloud-providers {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: var(--space-3);
		padding-top: var(--space-6);
		border-top: 1px solid var(--gray-200);
	}

	.cloud-label {
		font-size: var(--text-sm);
		color: var(--gray-500);
		font-weight: var(--font-medium);
	}

	.cloud-icons {
		display: flex;
		gap: var(--space-3);
	}

	.cloud-btn {
		width: 48px;
		height: 48px;
		border-radius: var(--radius-lg);
		border: 2px solid var(--gray-200);
		background: white;
		color: var(--gray-600);
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		transition: all var(--transition-fast);
	}

	.cloud-btn:hover {
		border-color: var(--primary-500);
		color: var(--primary-500);
		transform: translateY(-2px);
	}

	.file-input {
		display: none;
	}

	@media (max-width: 768px) {
		.upload-zone {
			padding: var(--space-8);
			min-height: 300px;
		}

		.upload-title {
			font-size: var(--text-xl);
		}
	}
</style>

<script lang="ts">
	import { 
		Header, 
		Footer, 
		UploadZone, 
		FileList, 
		TransferForm,
		CostSummary 
	} from '$lib/components';
	
	interface FileItem {
		id: string;
		file: File;
		progress?: number;
		status?: 'pending' | 'uploading' | 'complete' | 'error';
	}
	
	let selectedFiles = $state<FileItem[]>([]);
	let isSending = $state(false);
	
	function handleFilesSelected(files: File[]) {
		const newFiles: FileItem[] = files.map(file => ({
			id: crypto.randomUUID(),
			file,
			status: 'pending' as const
		}));
		selectedFiles = [...selectedFiles, ...newFiles];
	}
	
	function handleRemoveFile(id: string) {
		selectedFiles = selectedFiles.filter(f => f.id !== id);
	}
	
	function handleTransferSubmit(data: any) {
		console.log('Transfer data:', data);
		console.log('Files:', selectedFiles);
		// Handle transfer logic here
	}
	
	function handleSend() {
		if (selectedFiles.length === 0) return;
		
		isSending = true;
		
		// Simulate upload
		setTimeout(() => {
			isSending = false;
			// Navigate to success page or show confirmation
			alert('Files sent successfully!');
		}, 2000);
	}
	
	const totalSize = $derived(
		selectedFiles.reduce((sum, item) => sum + item.file.size, 0)
	);
	
	const estimatedCost = $derived(
		selectedFiles.length === 0 ? 0 : 0 // P2P is free
	);
</script>

<svelte:head>
	<title>New Transfer - Glimpse</title>
</svelte:head>

<div class="transfer-page">
	<Header />
	
	<main class="transfer-main">
		<div class="container-narrow">
			<!-- Page Header -->
			<div class="page-header">
				<h1 class="page-title">Send Files</h1>
				<p class="page-subtitle">
					Share files securely with end-to-end encryption. No account required.
				</p>
			</div>
			
			<!-- Upload Zone -->
			<UploadZone onFilesSelected={handleFilesSelected} />
			
			<!-- File List -->
			<FileList files={selectedFiles} onRemove={handleRemoveFile} />
			
			<!-- Transfer Form (only show if files are selected) -->
			{#if selectedFiles.length > 0}
				<TransferForm onSubmit={handleTransferSubmit} />
				
				<!-- Cost Summary -->
				<CostSummary
					connectionType="p2p"
					fileCount={selectedFiles.length}
					{totalSize}
					{estimatedCost}
					onSend={handleSend}
					{isSending}
				/>
			{/if}
		</div>
	</main>
	
	<Footer />
</div>

<style>
	.transfer-page {
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		background: var(--gray-50);
	}

	.transfer-main {
		flex: 1;
		padding: var(--space-12) 0 var(--space-20);
	}

	.container-narrow {
		max-width: 800px;
		margin: 0 auto;
		padding: 0 var(--space-6);
	}

	.page-header {
		text-align: center;
		margin-bottom: var(--space-12);
	}

	.page-title {
		font-size: var(--text-4xl);
		font-weight: var(--font-black);
		color: var(--gray-900);
		margin-bottom: var(--space-3);
	}

	.page-subtitle {
		font-size: var(--text-lg);
		color: var(--gray-600);
		max-width: 600px;
		margin: 0 auto;
	}

	@media (max-width: 768px) {
		.transfer-main {
			padding: var(--space-8) 0 var(--space-16);
		}

		.page-title {
			font-size: var(--text-2xl);
		}

		.page-subtitle {
			font-size: var(--text-base);
		}
	}
</style>

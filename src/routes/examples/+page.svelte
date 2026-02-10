<script lang="ts">
	import { Icon } from '$lib/components';
</script>

<div class="examples-page">
	<h1>Icon Usage Examples</h1>
	<p class="subtitle">Real-world examples of how to use icons in your Glimpse application</p>

	<!-- Example 1: Action Buttons -->
	<section class="example-section">
		<h2>Action Buttons</h2>
		<div class="example-grid">
			<button class="btn btn-primary">
				<Icon name="cloud-upload" size={20} />
				Upload Files
			</button>
			<button class="btn btn-success">
				<Icon name="check" size={20} />
				Confirm
			</button>
			<button class="btn btn-danger">
				<Icon name="trash" size={20} />
				Delete
			</button>
			<button class="btn btn-secondary">
				<Icon name="edit" size={20} />
				Edit
			</button>
		</div>
	</section>

	<!-- Example 2: Icon-Only Buttons -->
	<section class="example-section">
		<h2>Icon-Only Buttons</h2>
		<div class="example-grid">
			<button class="btn-icon" aria-label="Settings">
				<Icon name="sliders" />
			</button>
			<button class="btn-icon" aria-label="Notifications">
				<Icon name="bell" />
			</button>
			<button class="btn-icon" aria-label="Search">
				<Icon name="search" />
			</button>
			<button class="btn-icon" aria-label="More options">
				<Icon name="more-vertical" />
			</button>
		</div>
	</section>

	<!-- Example 3: Status Badges -->
	<section class="example-section">
		<h2>Status Badges</h2>
		<div class="example-grid">
			<div class="badge badge-success">
				<Icon name="check-circle" size={16} />
				Complete
			</div>
			<div class="badge badge-danger">
				<Icon name="x-circle" size={16} />
				Error
			</div>
			<div class="badge badge-warning">
				<Icon name="alert-triangle" size={16} />
				Warning
			</div>
			<div class="badge badge-info">
				<Icon name="info-circle" size={16} />
				Info
			</div>
		</div>
	</section>

	<!-- Example 4: Feature Cards -->
	<section class="example-section">
		<h2>Feature Cards</h2>
		<div class="example-grid cards">
			<div class="feature-card">
				<div class="feature-icon">
					<Icon name="shield-check" size={32} />
				</div>
				<h3>Secure Transfer</h3>
				<p>End-to-end encryption for all your files</p>
			</div>
			<div class="feature-card">
				<div class="feature-icon">
					<Icon name="zap" size={32} />
				</div>
				<h3>Lightning Fast</h3>
				<p>Optimized for maximum transfer speed</p>
			</div>
			<div class="feature-card">
				<div class="feature-icon">
					<Icon name="eye" size={32} />
				</div>
				<h3>Visual Preview</h3>
				<p>See files before downloading them</p>
			</div>
		</div>
	</section>

	<!-- Example 5: Form Inputs -->
	<section class="example-section">
		<h2>Form Inputs with Icons</h2>
		<div class="form-example">
			<div class="input-group">
				<Icon name="user" size={20} />
				<input type="text" placeholder="Username" />
			</div>
			<div class="input-group">
				<Icon name="mail" size={20} />
				<input type="email" placeholder="Email address" />
			</div>
			<div class="input-group">
				<Icon name="lock" size={20} />
				<input type="password" placeholder="Password" />
			</div>
			<div class="input-group">
				<Icon name="search" size={20} />
				<input type="search" placeholder="Search..." />
			</div>
		</div>
	</section>

	<!-- Example 6: File List -->
	<section class="example-section">
		<h2>File List Items</h2>
		<div class="file-list">
			<div class="file-item">
				<Icon name="file-pdf" size={24} />
				<div class="file-info">
					<div class="file-name">Annual_Report_2024.pdf</div>
					<div class="file-meta">2.4 MB • PDF Document</div>
				</div>
				<button class="btn-icon" aria-label="Remove">
					<Icon name="x" size={16} />
				</button>
			</div>
			<div class="file-item">
				<Icon name="file-image" size={24} />
				<div class="file-info">
					<div class="file-name">vacation_photo.jpg</div>
					<div class="file-meta">4.8 MB • JPEG Image</div>
				</div>
				<button class="btn-icon" aria-label="Remove">
					<Icon name="x" size={16} />
				</button>
			</div>
			<div class="file-item">
				<Icon name="file-code" size={24} />
				<div class="file-info">
					<div class="file-name">app.svelte</div>
					<div class="file-meta">12 KB • Svelte Component</div>
				</div>
				<button class="btn-icon" aria-label="Remove">
					<Icon name="x" size={16} />
				</button>
			</div>
		</div>
	</section>

	<!-- Example 7: Navigation -->
	<section class="example-section">
		<h2>Navigation Elements</h2>
		<div class="nav-example">
			<nav class="nav-menu">
				<a href="/" class="nav-item active">
					<Icon name="home" size={20} />
					Home
				</a>
				<a href="/files" class="nav-item">
					<Icon name="folder-open" size={20} />
					Files
				</a>
				<a href="/activity" class="nav-item">
					<Icon name="activity" size={20} />
					Activity
				</a>
				<a href="/settings" class="nav-item">
					<Icon name="sliders" size={20} />
					Settings
				</a>
			</nav>
		</div>
	</section>

	<!-- Example 8: Alert Messages -->
	<section class="example-section">
		<h2>Alert Messages</h2>
		<div class="alerts-example">
			<div class="alert alert-success">
				<Icon name="check-circle" size={24} />
				<div>
					<strong>Success!</strong> Your files have been uploaded successfully.
				</div>
			</div>
			<div class="alert alert-error">
				<Icon name="x-circle" size={24} />
				<div>
					<strong>Error!</strong> Failed to upload files. Please try again.
				</div>
			</div>
			<div class="alert alert-warning">
				<Icon name="alert-triangle" size={24} />
				<div>
					<strong>Warning!</strong> This link will expire in 24 hours.
				</div>
			</div>
			<div class="alert alert-info">
				<Icon name="info-circle" size={24} />
				<div>
					<strong>Info:</strong> Files are automatically scanned for viruses.
				</div>
			</div>
		</div>
	</section>

	<!-- Example 9: Stats Cards -->
	<section class="example-section">
		<h2>Statistics Cards</h2>
		<div class="example-grid cards">
			<div class="stat-card">
				<Icon name="cloud-upload" size={32} class="stat-icon" />
				<div class="stat-value">1,234</div>
				<div class="stat-label">Files Uploaded</div>
			</div>
			<div class="stat-card">
				<Icon name="users" size={32} class="stat-icon" />
				<div class="stat-value">456</div>
				<div class="stat-label">Active Users</div>
			</div>
			<div class="stat-card">
				<Icon name="trending-up" size={32} class="stat-icon" />
				<div class="stat-value">89%</div>
				<div class="stat-label">Success Rate</div>
			</div>
		</div>
	</section>
</div>

<style>
	.examples-page {
		max-width: 1200px;
		margin: 0 auto;
		padding: 2rem;
	}

	h1 {
		font-size: 2.5rem;
		font-weight: 700;
		margin-bottom: 0.5rem;
	}

	.subtitle {
		font-size: 1.125rem;
		color: #64748b;
		margin-bottom: 3rem;
	}

	.example-section {
		margin-bottom: 4rem;
	}

	.example-section h2 {
		font-size: 1.5rem;
		font-weight: 600;
		margin-bottom: 1.5rem;
		color: #1e293b;
	}

	.example-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
		gap: 1rem;
	}

	.example-grid.cards {
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
	}

	/* Buttons */
	.btn {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		padding: 0.75rem 1.5rem;
		border: none;
		border-radius: 8px;
		font-weight: 500;
		cursor: pointer;
		transition: all 0.2s;
	}

	.btn-primary {
		background: linear-gradient(135deg, #8b5cf6 0%, #6366f1 100%);
		color: white;
	}

	.btn-success {
		background: #10b981;
		color: white;
	}

	.btn-danger {
		background: #ef4444;
		color: white;
	}

	.btn-secondary {
		background: #f1f5f9;
		color: #475569;
	}

	.btn-icon {
		width: 40px;
		height: 40px;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 2px solid #e2e8f0;
		border-radius: 8px;
		background: white;
		cursor: pointer;
		transition: all 0.2s;
	}

	.btn-icon:hover {
		border-color: #8b5cf6;
		color: #8b5cf6;
	}

	/* Badges */
	.badge {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		padding: 0.5rem 1rem;
		border-radius: 6px;
		font-size: 0.875rem;
		font-weight: 500;
	}

	.badge-success {
		background: #dcfce7;
		color: #166534;
	}

	.badge-danger {
		background: #fee2e2;
		color: #991b1b;
	}

	.badge-warning {
		background: #fef3c7;
		color: #92400e;
	}

	.badge-info {
		background: #dbeafe;
		color: #1e40af;
	}

	/* Feature Cards */
	.feature-card {
		padding: 2rem;
		background: white;
		border: 2px solid #e2e8f0;
		border-radius: 12px;
		text-align: center;
		transition: all 0.2s;
	}

	.feature-card:hover {
		border-color: #8b5cf6;
		transform: translateY(-2px);
		box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
	}

	.feature-icon {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 64px;
		height: 64px;
		margin: 0 auto 1rem;
		background: linear-gradient(135deg, #8b5cf6 0%, #6366f1 100%);
		border-radius: 12px;
		color: white;
	}

	.feature-card h3 {
		font-size: 1.125rem;
		font-weight: 600;
		margin-bottom: 0.5rem;
	}

	.feature-card p {
		color: #64748b;
		font-size: 0.875rem;
	}

	/* Form Inputs */
	.form-example {
		display: flex;
		flex-direction: column;
		gap: 1rem;
		max-width: 400px;
	}

	.input-group {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		padding: 0.75rem 1rem;
		background: white;
		border: 2px solid #e2e8f0;
		border-radius: 8px;
		transition: all 0.2s;
	}

	.input-group:focus-within {
		border-color: #8b5cf6;
		box-shadow: 0 0 0 4px rgba(139, 92, 246, 0.1);
	}

	.input-group input {
		flex: 1;
		border: none;
		outline: none;
		font-size: 1rem;
	}

	/* File List */
	.file-list {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	.file-item {
		display: flex;
		align-items: center;
		gap: 1rem;
		padding: 1rem;
		background: white;
		border: 2px solid #e2e8f0;
		border-radius: 8px;
		transition: all 0.2s;
	}

	.file-item:hover {
		border-color: #cbd5e1;
	}

	.file-info {
		flex: 1;
	}

	.file-name {
		font-weight: 500;
		margin-bottom: 0.25rem;
	}

	.file-meta {
		font-size: 0.875rem;
		color: #64748b;
	}

	/* Navigation */
	.nav-menu {
		display: flex;
		gap: 0.5rem;
		padding: 0.5rem;
		background: white;
		border: 2px solid #e2e8f0;
		border-radius: 8px;
	}

	.nav-item {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		padding: 0.75rem 1rem;
		border-radius: 6px;
		text-decoration: none;
		color: #475569;
		font-weight: 500;
		transition: all 0.2s;
	}

	.nav-item:hover {
		background: #f1f5f9;
	}

	.nav-item.active {
		background: linear-gradient(135deg, #8b5cf6 0%, #6366f1 100%);
		color: white;
	}

	/* Alerts */
	.alerts-example {
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.alert {
		display: flex;
		gap: 1rem;
		padding: 1rem;
		border-radius: 8px;
		border-left: 4px solid;
	}

	.alert-success {
		background: #dcfce7;
		border-color: #10b981;
		color: #166534;
	}

	.alert-error {
		background: #fee2e2;
		border-color: #ef4444;
		color: #991b1b;
	}

	.alert-warning {
		background: #fef3c7;
		border-color: #f59e0b;
		color: #92400e;
	}

	.alert-info {
		background: #dbeafe;
		border-color: #3b82f6;
		color: #1e40af;
	}

	/* Stats Cards */
	.stat-card {
		padding: 2rem;
		background: white;
		border: 2px solid #e2e8f0;
		border-radius: 12px;
		text-align: center;
	}

	.stat-icon {
		color: #8b5cf6;
		margin-bottom: 1rem;
	}

	.stat-value {
		font-size: 2rem;
		font-weight: 700;
		color: #1e293b;
		margin-bottom: 0.5rem;
	}

	.stat-label {
		color: #64748b;
		font-size: 0.875rem;
	}

	@media (max-width: 768px) {
		.example-grid {
			grid-template-columns: 1fr;
		}

		h1 {
			font-size: 2rem;
		}
	}
</style>

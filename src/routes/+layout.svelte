<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import '../styles/main.css';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="" />
	<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet" />
	<title>Glimpse - See before you download</title>
	<meta name="description" content="AI-powered file transfer platform with intelligent preview and secure sharing." />
</svelte:head>

{@render children()}

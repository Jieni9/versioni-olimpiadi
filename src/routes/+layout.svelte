<script>
    import Nav from '$lib/components/Nav.svelte';
	import favicon from '$lib/assets/favicon.svg';
	import '../app.css';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>


<header class="top-bar">
	<p class="brand">Quante facce ha una medaglia?</p>
	<nav class="nav">
		<a href="/">Home</a>
		<a href="#about">About</a>
	</nav>
</header>

<style>
	.top-bar {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 2;
		display: flex;
		align-items: flex-start;
		justify-content: space-between;
		gap: 2rem;
		padding: 1.85rem clamp(1.5rem, 4.5vw, 3.25rem) 1rem;
		font-size: 0.8125rem;
		line-height: 1.35;
		font-weight: 500;
		background: linear-gradient(to bottom, rgb(255 255 255 / 98%), rgb(255 255 255 / 0%));
	}

	.brand {
		margin: 0;
		max-width: min(52vw, 17.5rem);
	}

	.nav {
		display: flex;
		gap: 1.65rem;
	}

	.nav a {
		color: inherit;
		text-decoration: none;
	}

</style>

{@render children()}

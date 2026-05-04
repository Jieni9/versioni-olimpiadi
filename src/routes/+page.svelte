<script lang="ts">
	import { onMount } from 'svelte';
	import Nav from '$lib/components/Nav.svelte';
	import { Canvas } from '@threlte/core';
	import Scene from '$lib/components/Scene.svelte';

	// Svelte 5 Runes
	let scrollY = $state(0);
	let maxScroll = $state(1);
	let iceSectionEl = $state<HTMLElement | null>(null);

	const iceParagraph =
		"Collaborazione dell'agenzia statunitense ICE per la sicurezza dei Giochi Invernali di Milano-Cortina 2026: compito di supporto tecnico e investigativo per la prevenzione di reati transnazionali, terrorismo e frodi legate all'evento.";

	const clamp01 = (value: number) => Math.max(0, Math.min(1, value));

	const updateMaxScroll = () => {
		const doc = document.documentElement;
		maxScroll = Math.max(1, doc.scrollHeight - window.innerHeight);
	};

	// Progress per Threlte
	let scrollProgress = $derived(clamp01(scrollY / maxScroll));

	onMount(() => {
		updateMaxScroll();
	});
</script>

<svelte:window bind:scrollY={scrollY} onresize={updateMaxScroll} />

<Nav />

<div class="canvas-layer" aria-hidden="true">
	<Canvas>
		<Scene progress={scrollProgress} />
	</Canvas>
</div>

<div class="page">
	<header class="top-bar">
		<p class="brand">Quante facce ha una medaglia?</p>
		<nav class="nav">
			<a href="/">Home</a>
			<a href="#about">About</a>
		</nav>
	</header>

	<main class="story">
		<section class="panel hero">
			<h1 class="hero-title">
				<span class="hero-line">Quante facce ha</span>
				<span class="hero-line">una medaglia?</span>
			</h1>
		</section>

		<!-- Sezione ICE modificata: no padding laterale, testo statico -->
		<section class="panel ice" bind:this={iceSectionEl} id="about">
			<div class="ice-frame">
				<p class="ice-copy">
					{iceParagraph}
				</p>
			</div>
		</section>
	</main>
</div>

<style>
	:global(html, body) {
		margin: 0;
		background: #ffffff;
		color: #000000;
		overflow-x: hidden;
	}

	.canvas-layer {
		position: fixed;
		inset: 0;
		z-index: 0;
		pointer-events: none;
		opacity: 0.55;
	}

	.page {
		position: relative;
		z-index: 1;
		min-height: 100vh;
	}

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

	.story {
		scroll-snap-type: y mandatory;
	}

	.panel {
		min-height: 100vh;
		min-height: 100svh;
		display: flex;
		align-items: center;
		justify-content: center;
		box-sizing: border-box;
		scroll-snap-align: start;
		scroll-snap-stop: always;
	}

	.hero-title {
		margin: 0;
		font-weight: 500;
		font-size: clamp(3.125rem, 10.25vw, 6.375rem);
		line-height: 1.02;
		letter-spacing: -0.012em;
		text-align: center;
	}

	.hero-line {
		display: block;
	}

.ice {
		min-height: 100vh;
		min-height: 100svh;
		/* Padding laterale zero per far andare il testo a bordo schermo */
		padding: 0; 
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.ice-frame {
		/* Larghezza quasi totale (95% dello schermo) */
		width: 95vw;
		max-width: 95vw; 
		margin: 0 auto;
		text-align: left;
	}

	.ice-copy {
		margin: 0;
		font-family: var(--font-display);
		font-size: clamp(2.5rem, 5vw, 4rem);
		line-height: 1.3;
		text-transform: none;
		word-spacing: -0.1em;
	}
</style>
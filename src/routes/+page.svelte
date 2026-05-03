<script lang="ts">
	import { onMount } from 'svelte';
	import { Canvas } from '@threlte/core';
	import Scene from '$lib/components/Scene.svelte';

	const iceParagraph =
		"Collaborazione dell'agenzia statunitense ICE per la sicurezza dei Giochi Invernali di Milano-Cortina 2026: compito di supporto tecnico e investigativo per la prevenzione di reati transnazionali, terrorismo e frodi legate all'evento.";

	let scrollY = $state(0);
	let maxScroll = $state(1);

	let iceSectionEl = $state<HTMLElement | null>(null);
	let typedText = $state('');
	let typingStarted = $state(false);

	const clamp01 = (value: number) => Math.max(0, Math.min(1, value));

	const updateMaxScroll = () => {
		const doc = document.documentElement;
		maxScroll = Math.max(1, doc.scrollHeight - window.innerHeight);
	};

	let scrollProgress = $derived(clamp01(scrollY / maxScroll));

	function runTypingAnimation() {
		let index = 0;
		const tick = () => {
			if (index > iceParagraph.length) return;
			typedText = iceParagraph.slice(0, index);
			index += 1;
			const char = iceParagraph[index - 1];
			const delay = char === ' ' ? 45 : char === ',' || char === ':' ? 120 : 28;
			window.setTimeout(tick, delay);
		};
		tick();
	}

	onMount(() => {
		updateMaxScroll();

		const el = iceSectionEl;
		if (!el) return;

		const observer = new IntersectionObserver(
			(entries) => {
				for (const entry of entries) {
					if (entry.isIntersecting && !typingStarted) {
						typingStarted = true;
						runTypingAnimation();
						observer.disconnect();
					}
				}
			},
			{ threshold: 0.45, rootMargin: '0px 0px -8% 0px' }
		);

		observer.observe(el);
		return () => observer.disconnect();
	});
</script>

<svelte:window bind:scrollY={scrollY} onresize={updateMaxScroll} />

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

		<section class="panel ice" bind:this={iceSectionEl} id="about">
			<div class="ice-frame">
				<p class="ice-copy">
					{typedText}<span
						class="caret"
						class:caret-active={typingStarted && typedText.length < iceParagraph.length}
					></span>
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
		font-family: var(--font-body);
		font-size: 0.8125rem; /* ~13px — matches small UI type in comp */
		line-height: 1.35;
		letter-spacing: 0;
		text-transform: none;
		font-weight: 500;
		background: linear-gradient(to bottom, rgb(255 255 255 / 98%), rgb(255 255 255 / 0%));
	}

	.brand {
		margin: 0;
		max-width: min(52vw, 17.5rem);
		line-height: 1.35;
		font-weight: 500;
	}

	.nav {
		display: flex;
		align-items: center;
		gap: 1.65rem;
		flex-shrink: 0;
		font-weight: 500;
	}

	.nav a {
		color: inherit;
		text-decoration: none;
	}

	.nav a:hover {
		text-decoration: underline;
		text-underline-offset: 3px;
	}

	.story {
		padding-top: 0;
		scroll-snap-type: y mandatory;
	}

	.panel {
		min-height: 100vh;
		min-height: 100svh;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: clamp(1.5rem, 5vw, 4rem);
		padding-left: clamp(1.5rem, 5vw, 3.25rem);
		padding-right: clamp(1.5rem, 5vw, 3.25rem);
		box-sizing: border-box;
		scroll-snap-align: start;
		scroll-snap-stop: always;
	}

	.hero {
		text-align: center;
	}

	.hero-title {
		margin: 0;
		font-family: var(--font-display);
		font-weight: 500;
		font-size: clamp(3.125rem, 10.25vw, 6.375rem); /* dominant headline scale */
		line-height: 1.02;
		letter-spacing: -0.012em;
		text-transform: none;
	}

	.hero-line {
		display: block;
	}

	/* Full second “screen”: one viewport tall, type centered as a block, copy left-aligned inside column */
	.ice {
		min-height: 100vh;
		min-height: 100svh;
		padding: clamp(4.75rem, 11vh, 6.5rem) clamp(1.5rem, 6vw, 4rem) clamp(2rem, 5vh, 3.5rem);
		align-items: center;
		justify-content: center;
	}

	.ice-frame {
		width: 100%;
		max-width: min(46rem, 90vw);
		margin-inline: auto;
		text-align: left;
	}

	.ice-copy {
		margin: 0;
		font-family: var(--font-display);
		font-size: clamp(1.875rem, 3.35vw, 2.875rem);
		line-height: 1.22;
		font-weight: 400;
		text-transform: none;
		letter-spacing: -0.018em;
	}

	.caret {
		display: inline-block;
		width: 0.075em;
		height: 0.85em;
		margin-left: 0.06em;
		vertical-align: -0.05em;
		background: transparent;
	}

	.caret-active {
		background: currentColor;
		animation: blink 0.95s steps(1, end) infinite;
	}

	@keyframes blink {
		50% {
			opacity: 0;
		}
	}
</style>

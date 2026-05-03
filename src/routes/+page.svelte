<script lang="ts">
	import { onMount } from 'svelte';
	import { Canvas } from '@threlte/core';
	import Scene from '$lib/components/Scene.svelte';

	let scrollY = $state(0);
	let maxScroll = $state(1);

	const clamp01 = (value: number) => Math.max(0, Math.min(1, value));

	const updateMaxScroll = () => {
		const doc = document.documentElement;
		maxScroll = Math.max(1, doc.scrollHeight - window.innerHeight);
	};

	let scrollProgress = $derived(clamp01(scrollY / maxScroll));

	onMount(() => {
		updateMaxScroll();
	});
</script>

<svelte:window bind:scrollY={scrollY} onresize={updateMaxScroll} />

<div class="canvas-layer">
	<Canvas>
		<Scene progress={scrollProgress} />
	</Canvas>
</div>

<main class="story">
	<section class="panel hero">
		<div class="content">
			<p class="kicker">Milano Cortina Preview</p>
			<h1>An Olympic medal opens the journey</h1>
			<p>
				The first landing section centers on a medal model, then the story expands into winter disciplines
				as you scroll.
			</p>
		</div>
	</section>

	<section class="panel chapter">
		<div class="content">
			<p class="kicker">Chapter 01</p>
			<h2>Podium focus</h2>
			<p>
				Gold-tone lighting and camera framing keep the medal dominant while introducing the editorial tone of
				the opening chapter.
			</p>
		</div>
	</section>

	<section class="panel chapter">
		<div class="content">
			<p class="kicker">Chapter 02</p>
			<h2>Winter events emerge</h2>
			<p>
				Ski, skate, and bobsled placeholders move in from the edges, hinting at the breadth of winter
				olympic disciplines around the central prize.
			</p>
		</div>
	</section>

	<section class="panel chapter">
		<div class="content">
			<p class="kicker">Chapter 03</p>
			<h2>Countdown to competition</h2>
			<p>
				The closing panel frames the medal and winter models as a final call to follow athletes into the next
				Olympic season.
			</p>
		</div>
	</section>
</main>

<style>
	:global(html, body) {
		margin: 0;
		background: #05070d;
		color: #f2f5ff;
	}

	.canvas-layer {
		position: fixed;
		inset: 0;
		z-index: 0;
	}

	.story {
		position: relative;
		z-index: 1;
	}

	.panel {
		min-height: 100vh;
		display: grid;
		align-items: center;
		padding: 8vw;
	}

	.content {
		max-width: 42rem;
		background: rgb(5 7 13 / 55%);
		border: 1px solid rgb(255 255 255 / 12%);
		padding: 1.5rem;
		backdrop-filter: blur(4px);
	}

	.kicker {
		margin: 0 0 0.75rem;
		text-transform: uppercase;
		letter-spacing: 0.12em;
		font-size: 0.75rem;
		opacity: 0.8;
	}

	h1,
	h2,
	p {
		margin: 0 0 0.85rem;
	}
</style>

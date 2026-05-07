<script lang="ts">
    import Nav from '$lib/components/Nav.svelte';
</script>

<Nav />

<main class="infra-page">
	<h1 class="infra-title">
		<span class="line-1">Le</span>
		<span class="line-2">infrastrutture</span>
	</h1>

	<p class="infra-description">
    Il Villaggio olimpico diventerà il più grande studentato d'Italia, con 1.700 posti letto a prezzi più accessibili rispetto a quelli del mercato privato.  
	</p>
</main>

<style>
	.infra-page {
		padding-top: 15vh;
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		padding-inline: 0;
		overflow-x: hidden; /* Evita scroll orizzontale dovuto al font gigante */
	}

	.infra-title {
		margin: 0;
		/* 280px = 17.5rem */
		font-size: clamp(4rem, 17.7vw, 17rem); 
		color: #F2571C;
		width: 100vw;
		line-height: 0.85; /* Interlinea stretta per compattare le due righe */
		letter-spacing: -0.05em;
		text-align: left;
		display: flex;
		flex-direction: column;
	}

	.line-1 {
		display: block;
	}

	.line-2 {
		display: block;
		/* Opzionale: un leggero margine negativo per avvicinare ancora di più le parole */
		margin-top: -0.05em; 
	}

	.infra-description {
		margin: 4rem 0 0 0;
		/* 96px = 6rem */
		font-size: clamp(2rem, 8vw, 6rem);
		width: 100vw;
		line-height: 1.05;
		letter-spacing: -0.03em;
		text-align: left;
        font-family: var(--font-display);

		/* Setup Gradiente Lineare */
		background: linear-gradient(
			to right, 
			#A2A5E2, 
			#F2571C, 
			#A2A5E2
		);
		-webkit-background-clip: text;
		background-clip: text;
		-webkit-text-fill-color: transparent;
		color: transparent;
		display: block;
	}
</style>
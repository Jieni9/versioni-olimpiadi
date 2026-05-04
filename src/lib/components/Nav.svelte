<script lang="ts">
	import { slide, fade } from 'svelte/transition';

	let isOpen = $state(false);
	const toggleMenu = () => (isOpen = !isOpen);

	const menuItems = [
		{ id: '01', label: 'Sostenibilità', href: '#sostenibilita' },
		{ id: '02', label: 'Sport', href: '#sport' },
		{ id: '03', label: 'Infrastrutture', href: '/infrastrutture' }
	];
</script>

<!-- Trigger con 3 linee -->
<button class="menu-trigger" onclick={toggleMenu} aria-label="Toggle Menu">
	{#if !isOpen}
		<div class="hamburger-icon">
			<span class="line"></span>
			<span class="line"></span>
			<span class="line"></span>
		</div>
	{:else}
		<span class="close-icon">&times;</span>
	{/if}
</button>

{#if isOpen}
	<div class="menu-overlay" transition:fade={{ duration: 200 }} onclick={toggleMenu}>
		<nav class="side-menu" transition:slide={{ axis: 'x', duration: 400 }} onclick={(e) => e.stopPropagation()}>
			<div class="menu-content">
				<ul class="main-links">
					{#each menuItems as item}
						<li>
							<a href={item.href} onclick={toggleMenu}>
								{item.id}/{item.label}
							</a>
						</li>
					{/each}
				</ul>

				<ul class="secondary-links">
					<li><a href="#about" onclick={toggleMenu}>About</a></li>
					<li><a href="#resources" onclick={toggleMenu}>Resources</a></li>
				</ul>
			</div>
		</nav>
	</div>
{/if}

<style>
.menu-trigger {
		position: fixed;
		top: 2.2rem; 
		z-index: 101;
		background: none;
		border: none;
		cursor: pointer;
		color: black;
		padding: 0;
		display: flex;
		align-items: center;
		height: 1.35;
        left: 20rem;
	}

	.hamburger-icon {
		display: flex;
		flex-direction: column;
		gap: 3px;
		width: 18px; 
	}

	.line {
		width: 100%;
		height: 1px; 
		background-color: currentColor;
	}

	.close-icon {
		font-size: 1.5rem; 
		line-height: 1;
	}

	/* Overlay */
	.menu-overlay {
		position: fixed;
		inset: 0;
		background: rgba(255, 255, 255, 0.05);
		z-index: 90;
	}

	/* Menu Fluttuante*/
	.side-menu {
		position: absolute;
		top: 6rem;          
		left: 1.5rem;       
		bottom: 2rem;        
		width: calc(33.33vw - 1.5rem); 
		backdrop-filter: blur(8px);
		display: flex;
		flex-direction: column;
		padding: 2.5rem;
		box-sizing: border-box;
		background: white;
	}

	.menu-content {
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		align-items: flex-end; /* Allineamento a destra */
		text-align: right;
	}

	ul {
		list-style: none;
		padding: 0;
		margin: 0;
		width: 100%;
	}


	.main-links a, .secondary-links a {
		font-size: clamp(32px, 4vw, 40px);
		text-decoration: none;
		display: block;
		line-height: 1.3;
		letter-spacing: -0.02em;
        font-family: var(--font-display);
        color: black;
	}

	.main-links {
		margin-top: 15vh;
	}

	.secondary-links {
		margin-bottom: 1rem;
	}

	a:hover {
		opacity: 0.5;
	}
</style>
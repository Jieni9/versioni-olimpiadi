<script lang="ts">
	import { T, useScene } from '@threlte/core';
	import { GLTF, OrbitControls, useDraco } from '@threlte/extras';
	import { Color } from 'three';

	const { scene } = useScene();

	// Imposta lo sfondo bianco
	$effect.pre(() => {
		const previous = scene.background;
		scene.background = new Color('#ffffff');
		return () => {
			scene.background = previous;
		};
	});

	let { progress = 0 }: { progress: number } = $props();

	/** Supporto per GLB compressi con Draco */
	const dracoLoader = useDraco();

	let medalGltfFailed = $state(false);

	const mix = (from: number, to: number, t: number) => from + (to - from) * t;

	// Movimenti della camera basati sullo scroll
	let cameraPosition: [number, number, number] = $derived([
		mix(-1.6, 2.2, progress),
		mix(1.2, 0.4, progress),
		mix(9.5, 5.2, progress)
	]);

	// Trasformazioni del modello basate sullo scroll
	let medalRotation: [number, number, number] = $derived([
		mix(0.15, 0.05, progress),
		progress * Math.PI * 1.1,
		0
	]);
	let medalPosition: [number, number, number] = $derived([0, mix(-0.2, 0.5, progress), 0]);
	let medalScale: number = $derived(mix(1.45, 1.1, progress));
</script>

<T.PerspectiveCamera makeDefault fov={45} position={cameraPosition} />
<OrbitControls enableZoom={false} enablePan={false} />

<T.AmbientLight intensity={0.72} />
<T.DirectionalLight position={[4, 6, 3]} intensity={0.95} />
<T.DirectionalLight position={[-4, 2, -2]} intensity={0.35} color="#c8d8ff" />

<!-- Gruppo principale che contiene solo il tuo modello zoh2026.glb -->
<T.Group rotation={medalRotation} position={medalPosition} scale={0.05}> 
	{#if !medalGltfFailed}
		<GLTF
			url="/models/zoh2026.glb"
			/* Prova a commentare dracoLoader se non sei sicuro sia compresso */
			/* dracoLoader={dracoLoader} */ 
			onerror={() => {
				medalGltfFailed = true;
			}}
		/>
	{:else}
		<T.Mesh>
			<T.IcosahedronGeometry args={[1, 1]} />
			<T.MeshStandardMaterial color="red" />
		</T.Mesh>
	{/if}
</T.Group>


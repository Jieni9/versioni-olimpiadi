<script lang="ts">
	import { T, useScene } from '@threlte/core';
	import { GLTF, OrbitControls, useDraco } from '@threlte/extras';
	import { Color } from 'three';

	const { scene } = useScene();

	$effect.pre(() => {
		const previous = scene.background;
		scene.background = new Color('#ffffff');
		return () => {
			scene.background = previous;
		};
	});

	let { progress = 0 }: { progress: number } = $props();

	/** Draco-compressed GLBs need this (common Blender export). */
	const dracoLoader = useDraco();

	let medalGltfFailed = $state(false);

	const mix = (from: number, to: number, t: number) => from + (to - from) * t;

	let cameraPosition: [number, number, number] = $derived([
		mix(-1.6, 2.2, progress),
		mix(1.2, 0.4, progress),
		mix(9.5, 5.2, progress)
	]);
	let medalRotation: [number, number, number] = $derived([
		mix(0.15, 0.05, progress),
		progress * Math.PI * 1.1,
		0
	]);
	let medalPosition: [number, number, number] = $derived([0, mix(-0.2, 0.5, progress), 0]);
	let medalScale: number = $derived(mix(1.45, 1.1, progress));

	let winterReveal = $derived(Math.max(0, (progress - 0.2) / 0.8));
	let winterSpin = $derived(winterReveal * Math.PI * 1.4);

	let skiPosition: [number, number, number] = $derived([
		mix(-5.4, -2.9, winterReveal),
		mix(-0.8, 0.45, winterReveal),
		mix(-2.4, -0.8, winterReveal)
	]);
	let skatePosition: [number, number, number] = $derived([
		mix(5.3, 2.8, winterReveal),
		mix(-0.7, 0.2, winterReveal),
		mix(-2.2, -0.4, winterReveal)
	]);
	let bobsledPosition: [number, number, number] = $derived([
		mix(0.4, 0.1, winterReveal),
		mix(-3.6, -1.35, winterReveal),
		mix(-3.8, -1.3, winterReveal)
	]);
</script>

<T.PerspectiveCamera makeDefault fov={45} position={cameraPosition} />
<OrbitControls enableZoom={false} enablePan={false} />

<T.AmbientLight intensity={0.72} />
<T.DirectionalLight position={[4, 6, 3]} intensity={0.95} />
<T.DirectionalLight position={[-4, 2, -2]} intensity={0.35} color="#c8d8ff" />

<T.Group rotation={medalRotation} position={medalPosition} scale={medalScale}>
	{#if !medalGltfFailed}
		<GLTF
			url="/models/zoh2026.glb"
			dracoLoader={dracoLoader}
			onerror={() => {
				medalGltfFailed = true;
				console.warn(
					'Could not load /models/zoh2026.glb — put the file at static/models/zoh2026.glb and reload.'
				);
			}}
		/>
	{:else}
		<T.Mesh position={[0, 0, 0]}>
			<T.IcosahedronGeometry args={[1.2, 1]} />
			<T.MeshStandardMaterial color="#f6c54a" roughness={0.25} metalness={0.75} />
		</T.Mesh>
	{/if}
	<T.Mesh position={[0, -0.05, 0]}>
		<T.CylinderGeometry args={[0.95, 0.95, 0.15, 48]} />
		<T.MeshStandardMaterial color="#f6c54a" roughness={0.2} metalness={0.82} />
	</T.Mesh>
</T.Group>

<T.Group position={skiPosition} rotation={[0, -winterSpin, 0]} scale={mix(0.5, 0.95, winterReveal)}>
	<T.Mesh>
		<T.BoxGeometry args={[0.34, 1.75, 0.34]} />
		<T.MeshStandardMaterial
			color="#7ed1ff"
			roughness={0.38}
			metalness={0.38}
			transparent
			opacity={mix(0.22, 0.5, winterReveal)}
		/>
	</T.Mesh>
</T.Group>

<T.Group position={skatePosition} rotation={[0.15, winterSpin * 0.8, 0]} scale={mix(0.5, 0.9, winterReveal)}>
	<T.Mesh>
		<T.TorusGeometry args={[0.6, 0.18, 16, 30]} />
		<T.MeshStandardMaterial
			color="#d0f1ff"
			roughness={0.5}
			metalness={0.28}
			transparent
			opacity={mix(0.15, 0.42, winterReveal)}
		/>
	</T.Mesh>
</T.Group>

<T.Group position={bobsledPosition} rotation={[0, -winterSpin * 1.05, 0]} scale={mix(0.45, 0.85, winterReveal)}>
	<T.Mesh>
		<T.CapsuleGeometry args={[0.38, 1.15, 10, 18]} />
		<T.MeshStandardMaterial
			color="#9fb7ff"
			roughness={0.42}
			metalness={0.36}
			transparent
			opacity={mix(0.18, 0.45, winterReveal)}
		/>
	</T.Mesh>
</T.Group>

<T.Mesh position={[0, -2.2, -0.6]} rotation={[-Math.PI / 2, 0, 0]}>
	<T.CircleGeometry args={[6.2, 48]} />
	<T.MeshStandardMaterial color="#1b2238" roughness={1} metalness={0} />
</T.Mesh>

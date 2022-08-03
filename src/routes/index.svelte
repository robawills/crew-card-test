<script lang="ts">
	import * as THREE from 'three';
	import * as SC from 'svelte-cubed';
	import { GLTFLoader, type GLTF } from 'three/examples/jsm/loaders/GLTFLoader.js';
	import { onMount } from 'svelte';

	let width = 1;
	let height = 1.5;
	let depth = 0.01;

	let spin = 0;

	SC.onFrame(() => {
		spin += 0.01;
	});

	let gltfModels: GLTF[] | null = null;

	async function loadAllGLTF() {
		const loader = new GLTFLoader();

		const modelList = await Promise.all([loader.loadAsync('crew-card.glb')]);

		gltfModels = modelList;
	}

	onMount(loadAllGLTF);
</script>

<SC.Canvas
	antialias
	background={new THREE.Color('#141517')}
	fog={new THREE.FogExp2('#141517', 0.1)}
	shadows
>
	<SC.Group position={[0, -height / 2, 0]}>
		<SC.Mesh
			geometry={new THREE.PlaneGeometry(50, 50)}
			material={new THREE.MeshStandardMaterial({ color: 0x1c1d20 })}
			rotation={[-Math.PI / 2, 0, 0]}
			receiveShadow
		/>
	</SC.Group>

	<SC.AmbientLight color={new THREE.Color(0xf7b5bc)} intensity={0.75} />
	{#if gltfModels}
		<SC.Primitive
			object={gltfModels[0].scene}
			position={[0, 0, 0]}
			rotation={[0, spin, 0.15]}
			scale={[0.5, 0.5, 0.5]}
		/>
	{/if}

	<SC.PerspectiveCamera position={[0, 0, 4]} />
	<SC.OrbitControls enableZoom={false} maxPolarAngle={Math.PI * 0.51} />
	<SC.AmbientLight intensity={0.6} />
	<SC.DirectionalLight intensity={0.6} position={[-2, 3, 2]} shadow={{ mapSize: [2048, 2048] }} />
</SC.Canvas>

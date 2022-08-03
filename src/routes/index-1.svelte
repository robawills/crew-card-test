<script>
	import * as THREE from 'three';
	import * as SC from 'svelte-cubed';
	import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
	import { onMount } from 'svelte';

	let gltfModels = null;

	function loadAllGLTF() {
		const loader = new GLTFLoader();
		Promise.all([
			loader.loadAsync(
				'https://raw.githubusercontent.com/mrdoob/three.js/dev/examples/models/gltf/LeePerrySmith/LeePerrySmith.glb'
			)
		]).then((modelList) => (gltfModels = modelList));
	}

	let spin = 0;

	SC.onFrame(() => {
		spin += 0.01;
	});

	onMount(() => {
		loadAllGLTF();
	});
</script>

<!-- TEMPLATE -->

<SC.Canvas
	background={new THREE.Color('#141517')}
	shadows={true}
	antialias={true}
	physicallyCorrectLights={false}
>
	<SC.PerspectiveCamera position={[1, 3, 8]} near={0.1} far={400} fov={55} />

	<SC.OrbitControls
		enabled={true}
		enableZoom={false}
		autoRotate={false}
		autoRotateSpeed={2}
		enableDamping={true}
		dampingFactor={0.1}
	/>

	<SC.DirectionalLight
		color={new THREE.Color(0xf7b5bc)}
		position={[0, 10, 10]}
		intensity={0.75}
		shadow={false}
	/>

	<SC.AmbientLight color={new THREE.Color(0xf7b5bc)} intensity={0.75} />
	{#if gltfModels}
		<SC.Primitive
			object={gltfModels[0].scene}
			position={[0, 0, 0]}
			rotation={[0, spin, 0.15]}
			scale={[0.5, 0.5, 0.5]}
		/>
	{/if}
</SC.Canvas>

<script lang="ts">
  import { onMount } from "svelte";
  import type { Mesh, MeshBasicMaterial, PerspectiveCamera, Scene, TorusGeometry, WebGL1Renderer } from "three";
  import * as THREE from "three";
  import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
  import DebugScene from "./DebugScene.svelte";

  export let debug = false;
  let showDebug;

  let canvas: HTMLCanvasElement;
  let scene: Scene;
  let camera: PerspectiveCamera;
  let renderer: WebGL1Renderer;
  let orbitControls: OrbitControls;

  const fov = 50;
  const near = 0.01;
  const far = 10000;

  onMount(() => {
    let width = canvas.parentElement.clientWidth;
    let height = canvas.parentElement.clientHeight;
    scene = new THREE.Scene();
    renderer = new THREE.WebGL1Renderer({ canvas });
    camera = new THREE.PerspectiveCamera(fov, width / height, near, far);
    orbitControls = new OrbitControls(camera, renderer.domElement);

    scene.background = new THREE.Color(0x000040);

    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(width, height);

    camera.position.setZ(50);
    camera.updateProjectionMatrix();

    createTorus();

    showDebug = !!debug;
    animate();
  });

  function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
  }

  function createTorus() {
    const geometry: TorusGeometry = new THREE.TorusGeometry(10, 3, 16, 100);
    const material: MeshBasicMaterial = new THREE.MeshBasicMaterial({
      color: 0xff6347,
      wireframe: true,
    });
    const torus: Mesh = new THREE.Mesh(geometry, material);
    scene.add(torus);
  }
</script>

<canvas id="canvas" bind:this="{canvas}"></canvas>
{#if showDebug}
  <DebugScene camera="{camera}" scene="{scene}" />
{/if}

<style>
  #canvas {
    width: 100%;
    height: 100%;
  }
</style>

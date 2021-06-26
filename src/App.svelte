<script lang="ts">
  import { onMount } from "svelte";
  import type { CameraHelper, Mesh, MeshBasicMaterial, PerspectiveCamera, Scene, TorusGeometry, WebGLRenderer } from "three";

  import * as THREE from "three";
  import Info from "./lib/Info.svelte";

  let canvas: HTMLCanvasElement;
  let value: number;

  let mouse = {
    x: 0,
    y: 0,
  };

  let screen = {
    width: window.innerWidth,
    height: window.innerHeight,
  };

  const scene: Scene = new THREE.Scene();
  const camera: PerspectiveCamera = new THREE.PerspectiveCamera(75, screen.width / screen.height, 0.01, 1000);
  let renderer: WebGLRenderer;

  const cameraHelper: CameraHelper = new THREE.CameraHelper(camera);
  scene.add(cameraHelper);

  const geometry: TorusGeometry = new THREE.TorusGeometry(10, 3, 16, 100);
  const material: MeshBasicMaterial = new THREE.MeshBasicMaterial({
    color: 0xff6347,
    wireframe: true,
  });
  const torus: Mesh = new THREE.Mesh(geometry, material);

  scene.add(torus);

  onMount(() => {
    window.onscroll = (e: Event) => console.log(e);
    renderer = new THREE.WebGLRenderer({ canvas });

    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(screen.width, screen.height);
    camera.position.setZ(30);

    function animate() {
      requestAnimationFrame(animate);
      torus.rotation.x += 0.01;
      renderer.render(scene, camera);
    }
    animate();
  });

  function handleMouseMove(event: MouseEvent) {
    mouse.x = event.x;
    mouse.y = event.y;
    // camera.position.set(mouse.x, mouse.y, 30);
  }

  function handleResize(event: Event) {
    const target = event.currentTarget as Window;
    screen.width = target.innerWidth;
    screen.height = target.innerHeight;
    camera.aspect = screen.width / screen.height;
    camera.updateProjectionMatrix();
    renderer.setSize(screen.width, screen.height);
  }

  function handleWheel(event: WheelEvent) {
    if (event.deltaY > 0) {
      camera.zoom = camera.zoom > 10 ? 10 : camera.zoom + 0.05;
    } else {
      camera.zoom = camera.zoom < 0 ? 0 : camera.zoom - 0.05;
    }
    camera.updateProjectionMatrix();
  }

  function handleMouseDown(event: Event) {
    console.log(event);
  }
</script>

<svelte:window on:resize="{handleResize}" on:wheel="{handleWheel}" on:mousedown="{handleMouseDown}" />

<div class="info">
  <Info title="Mouse" object="{mouse}" />
  <Info title="Screen" object="{screen}" />
</div>

<canvas bind:this="{canvas}" on:mousemove="{handleMouseMove}"></canvas>

<style>
  .info {
    z-index: 1;
    display: inline-block;
    position: absolute;
    top: 10;
    left: 10;
  }

  canvas {
    position: fixed;
    top: 0;
    left: 0;
  }
</style>

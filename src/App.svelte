<script lang="ts">
  import { onMount } from "svelte";
  import type { CameraHelper, Mesh, MeshBasicMaterial, PerspectiveCamera, Scene, TorusGeometry, WebGLRenderer } from "three";
  import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
  import * as THREE from "three";
  import Info from "./lib/Info.svelte";

  let canvas: HTMLCanvasElement;
  let canvas2: HTMLCanvasElement;
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
  const camera: PerspectiveCamera = new THREE.PerspectiveCamera(60, screen.width / screen.height, 10, 1000);
  const cameraHelper: CameraHelper = new THREE.CameraHelper(camera);
  scene.background = new THREE.Color(0x000040);
  scene.add(cameraHelper);

  const camera2: PerspectiveCamera = new THREE.PerspectiveCamera(60, screen.width / screen.height, 0.01, 10000);

  let renderer: WebGLRenderer;
  let renderer2: WebGLRenderer;

  const geometry: TorusGeometry = new THREE.TorusGeometry(10, 3, 16, 100);
  const material: MeshBasicMaterial = new THREE.MeshBasicMaterial({
    color: 0xff6347,
    wireframe: true,
  });
  const torus: Mesh = new THREE.Mesh(geometry, material);
  scene.add(torus);

  onMount(() => {
    renderer = new THREE.WebGLRenderer({ canvas });
    renderer2 = new THREE.WebGLRenderer({ canvas: canvas2 });
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(screen.width, screen.height / 2);
    renderer2.setPixelRatio(window.devicePixelRatio);
    renderer2.setSize(screen.width, screen.height / 2);
    camera.position.setZ(30);
    camera2.position.setZ(100);
    camera.updateProjectionMatrix();
    camera2.updateProjectionMatrix();
    const orbitControls: OrbitControls = new OrbitControls(camera, renderer.domElement);
    orbitControls.update();
    const orbitControls2: OrbitControls = new OrbitControls(camera2, renderer2.domElement);
    orbitControls2.update();
    function animate() {
      requestAnimationFrame(animate);
      torus.rotation.x += 0.01;
      cameraHelper.visible = false;
      renderer.render(scene, camera);
      cameraHelper.visible = true;
      renderer2.render(scene, camera2);
    }
    animate();
  });

  function handleMouseMove(event: MouseEvent) {
    mouse.x = event.x;
    mouse.y = event.y;
    // camera.position.set(mouse.x, mouse.y, 30);
  }

  function handleResize(event: Event) {
    // const target = event.currentTarget as Window;
    // screen.width = target.innerWidth;
    // screen.height = target.innerHeight;
    // camera.aspect = screen.width / screen.height;
    // camera.updateProjectionMatrix();
    // renderer.setSize(screen.width, screen.height);
  }

  function handleWheel(event: WheelEvent) {
    // if (event.deltaY > 0) {
    //   camera.zoom = camera.zoom > 10 ? 10 : camera.zoom + 0.05;
    // } else {
    //   camera.zoom = camera.zoom < 0 ? 0 : camera.zoom - 0.05;
    // }
    // camera.updateProjectionMatrix();
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
<div class="flex">
  <div>
    <canvas bind:this="{canvas}" on:mousemove="{handleMouseMove}"></canvas>
  </div>
  <div>
    <canvas bind:this="{canvas2}"></canvas>
  </div>
</div>

<style>
  .info {
    z-index: 1;
    display: flex;
    position: absolute;
    top: 10;
    left: 10;
  }
  .flex {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
  }
  .flex > * {
    width: 100%;
    height: 100%;
  }
</style>

<script lang="ts">
  import { onMount } from "svelte";
  import * as THREE from "three";
  import type { CameraHelper, OrthographicCamera, PerspectiveCamera, Scene, WebGL1Renderer } from "three";
  import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

  export let scene: Scene;
  export let camera: PerspectiveCamera | OrthographicCamera;
  export let fov = 50;

  let canvas: HTMLCanvasElement;
  let renderer: WebGL1Renderer;
  let cameraHelper: CameraHelper;
  let debugCamera: PerspectiveCamera;
  let orbitControls: OrbitControls;

  onMount(() => {
    let width = canvas.parentElement.clientWidth;
    let height = canvas.parentElement.clientHeight;
    const near = 0.01;
    const far = 10000;
    renderer = new THREE.WebGL1Renderer({ canvas });
    debugCamera = new THREE.PerspectiveCamera(fov, width / height, near, far);
    cameraHelper = new THREE.CameraHelper(camera);
    orbitControls = new OrbitControls(debugCamera, renderer.domElement);

    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(screen.width, screen.height / 2);

    debugCamera.position.setZ(100);
    debugCamera.updateProjectionMatrix();

    animate();
  });

  function animate() {
    requestAnimationFrame(animate);
    cameraHelper.visible = true;
    renderer.render(scene, debugCamera);
  }
</script>

<canvas bind:this="{canvas}"></canvas>

<style>
</style>

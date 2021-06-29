<script lang="ts">
  import { onMount } from "svelte";
  import type { Camera, OrthographicCamera, PerspectiveCamera, Scene } from "three";
  import DebugScene from "./lib/DebugScene.svelte";
  import MainScene from "./lib/MainScene.svelte";

  let debug = true;
</script>

<div class="flex-column">
  <div class="main-scene">
    <MainScene debug="true" />
  </div>
  <div class="main-scene"></div>
</div>

<style>
  .flex-column {
    display: flex;
    flex-direction: column;
    height: 100%;
  }
  .main-scene {
    flex: 1;
    background-color: red;
  }
</style>

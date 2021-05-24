<template>
  <div>
    <canvas class="webgl"></canvas>
  </div>
</template>

<script>
import * as THREE from "three";
export default {
  methods: {
    makeScene() {
      const scene = new THREE.Scene();
      const geometry = new THREE.BoxGeometry(2, 2, 2);
      const material = new THREE.MeshBasicMaterial({ color: "lightblue" });
      const mesh = new THREE.Mesh(geometry, material);
      scene.add(mesh);
      //Camera
      const camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight
      );
      camera.position.x = 1;
      camera.position.z = 3;
      scene.add(camera);

      //Renderer
      const renderer = new THREE.WebGLRenderer({
        canvas: document.querySelector(".webgl"),
      });
      renderer.setSize(window.innerWidth, window.innerHeight);

      renderer.render(scene, camera);
    },
  },
  mounted() {
      this.makeScene()
  },
};
</script>

<style scoped>
</style>
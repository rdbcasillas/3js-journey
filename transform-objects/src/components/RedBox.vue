<template>
  <div>
    <div class="slidecontainer">
      <label for="myRange">X and Y Scale </label>
      <input
        type="range"
        min="0"
        max="4"
        step="0.1"
        :value="xscale"
        class="slider"
        id="myRange"
        @input="onChg($event)"
      />
      <br>
      <label for="myRot">X Rot </label>
      <input
        type="range"
        min="0"
        max="4"
        step="0.1"
        :value="xrot"
        class="rotslider"
        @input="onChgxrot($event)"
        id="myRot"
      />
      <label for="myRot">Y Rot </label>
      <input
        type="range"
        min="0"
        max="4"
        step="0.1"
        :value="yrot"
        class="rotslider"
        @input="onChgyrot($event)"
        id="myRot"
      />
      <p></p>
    </div>
    <canvas class="webgl"></canvas>
  </div>
</template>

<script>
import * as THREE from "three";
export default {
  data() {
    return {
      x: 1.9,
      y: 0.8,
      xscale: 2,
      yscale: 2,
      xrot: Math.PI,
      yrot: Math.PI,
      width: 800,
      height: 800,
      //sliderval:1
    };
  },
  methods: {
    makeScene() {
      const scene = new THREE.Scene();
      const geometry = new THREE.BoxGeometry(2, 2, 2);
      const material = new THREE.MeshBasicMaterial({ color: "lightblue" });
      const mesh = new THREE.Mesh(geometry, material);
      mesh.position.x = this.x;
      mesh.position.y = this.y;

      mesh.scale.x = this.xscale 
      mesh.scale.y = this.yscale 
      mesh.rotation.x = this.xrot
      mesh.rotation.y = this.yrot
      scene.add(mesh);
      //Camera
      const camera = new THREE.PerspectiveCamera(75, this.width / this.height);
      camera.position.x = 1;
      camera.position.z = 5;
      scene.add(camera);

      //Renderer
      const renderer = new THREE.WebGLRenderer({
        canvas: document.querySelector(".webgl"),
      });
      renderer.setSize(this.width, this.height);

      renderer.render(scene, camera);
    },
    onChg(e) {
      this.xscale = e.target.value;
      this.yscale = e.target.value;
    },
    onChgxrot(e) {
      this.xrot = e.target.value;
    },
    onChgyrot(e) {
      this.yrot = e.target.value;
    },
  },
  mounted() {
    this.makeScene();
  },
  watch: {
    xscale: function (val) {
      this.makeScene();
    },
    xrot: function (val) {
      this.makeScene();
    },
    yrot: function (val) {
      this.makeScene();
    },
  },
};
</script>

<style scoped>
</style>
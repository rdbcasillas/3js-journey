<template>
  <div>
    <canvas class="webgl"></canvas>
  </div>
</template>

<script>
import * as THREE from "three";
import gsap from "gsap";
import * as dat from "dat.gui";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import myimage from "../assets/leather.jpeg"
const scene = new THREE.Scene();
//const gui = new dat.GUI();
const textureLoader = new THREE.TextureLoader();
const texture = textureLoader.load(myimage) 
export default {
  data() {
    return {
      width: window.innerWidth,
      height: window.innerHeight,
      cursor: {
        x: 0,
        y: 0,
      },
      parameters: {
        color: "#008000",
        spin: (obj) => {
          gsap.to(obj.rotation, { duration: 1, y: obj.rotation.y + 10 });
        },
      },
      camera: {},
      renderObj: {},
      canv: {},
    };
  },
  methods: {
    makeScene() {
      let that = this;
      const canvas = document.querySelector(".webgl");
      this.canv = canvas;
      const geometry = new THREE.BoxGeometry(2, 2, 2, 2, 2, 2);
      const material = new THREE.MeshBasicMaterial({
        map: texture,
      });

      const mesh = new THREE.Mesh(geometry, material);

      scene.add(mesh);

      gui.add(mesh.position, "z").min(-3).max(3).step(0.01).name("cube Y");

      gui.addColor(this.parameters, "color").onChange(() => {
        material.color.set(that.parameters.color);
      });

      gui.add({ spin: this.parameters.spin.bind(this, mesh) }, "spin");
      //Camera
      this.camera = new THREE.PerspectiveCamera(75, this.width / this.height);
      this.camera.position.x = 1;
      this.camera.position.z = 10;

      //orbit control for mouse interaction
      const controls = new OrbitControls(this.camera, canvas);
      controls.enableDamping = true;
      scene.add(this.camera);

      //Renderer
      const renderer = new THREE.WebGLRenderer({
        canvas,
      });
      renderer.setSize(this.width, this.height);
      renderer.setPixelRatio(window.devicePixelRatio);
      this.renderObj = renderer;
      const clock = new THREE.Clock();
      function animate() {

        const elapsedTime = clock.getElapsedTime();
        controls.update()
        renderer.render(scene, that.camera);
        requestAnimationFrame(animate);
      }
      animate();
    },
  },
  mounted() {
    window.addEventListener("mousemove", (event) => {
      this.cursor.x = event.clientX / 800 - 0.5;
      this.cursor.y = event.clientY / 800 - 0.5;
    });
    window.addEventListener("resize", () => {
      this.width = window.innerWidth;
      this.height = window.innerHeight;

      this.camera.aspect = this.width / this.height;
      this.camera.updateProjectionMatrix();
      this.renderObj.setSize(this.width, this.height);
    });
    window.addEventListener("dblclick", () => {
      if (!document.fullscreenElement) {
        console.log(this.canv);
        this.canv.requestFullscreen();
      } else {
        document.exitFullscreen();
      }
    });
    this.makeScene();
  },
};
</script>

<style scoped>
.webgl {
  position: fixed;
  top: 0;
  left: 0;
  background-color: whitesmoke;
}
</style>
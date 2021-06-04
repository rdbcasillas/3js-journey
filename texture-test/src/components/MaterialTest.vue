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
import myfont from "../assets/fonts/helvetiker_regular.typeface.json";
const scene = new THREE.Scene();
/**
 * Textures
 */
const textureLoader = new THREE.TextureLoader();

const doorColorTexture = textureLoader.load(
  "../../src/assets/textures/door/color.jpg"
);
const doorAlphaTexture = textureLoader.load(
  "../../src/assets/textures/door/alpha.jpg"
);
const doorAmbientOcclusionTexture = textureLoader.load(
  "../../src/assets/textures/door/ambientOcclusion.jpg"
);
const doorHeightTexture = textureLoader.load(
  "../../src/assets/textures/door/height.jpg"
);
const doorNormalTexture = textureLoader.load(
  "../../src/assets/textures/door/normal.jpg"
);
const doorMetalnessTexture = textureLoader.load(
  "../../src/assets/textures/door/metalness.jpg"
);
const doorRoughnessTexture = textureLoader.load(
  "../../src/assets/textures/door/roughness.jpg"
);
const matcapTexture = textureLoader.load(
  "../../src/assets/textures/matcaps/8.png"
);
const gradientTexture = textureLoader.load(
  "../../src/assets/textures/gradients/3.jpg"
);

const gui = new dat.GUI();
export default {
  data() {
    return {
      width: window.innerWidth,
      height: window.innerHeight,
      camera: {},
    };
  },
  methods: {
    addScene() {
      const canvas = document.querySelector(".webgl");
      //      const material = new THREE.MeshBasicMaterial();
      const material = new THREE.MeshStandardMaterial();
      material.roughness = 0.45;
      material.metalness = 0.65;
      material.map = doorColorTexture;

      gui.add(material, "metalness").min(0).max(1).step(0.0001);
      gui.add(material, "roughness").min(0).max(1).step(0.0001);
      const sphere = new THREE.Mesh(
        new THREE.SphereGeometry(1.5, 16, 16),
        material
      );
      sphere.position.x = -8;

      const torus = new THREE.Mesh(
        new THREE.TorusGeometry(1, 0.5, 16, 32),
        material
      );
      torus.position.x = 8;

      scene.add(sphere, torus);

      // Font stuff
      const fontLoader = new THREE.FontLoader();

      fontLoader.load(
        "../../src/assets/fonts/helvetiker_regular.typeface.json",
        (font) => {
          const textGeometry = new THREE.TextGeometry("All the best Ish!", {
            font: font,
            size: 0.9,
            height: 0.4,
            curveSegments: 6,
            bevelEnabled: true,
            bevelThickness: 0.03,
            bevelSize: 0.02,
            bevelOffset: 0,
            bevelSegments: 4,
          });
          const textMaterial = new THREE.MeshMatcapMaterial({
            matcap: matcapTexture,
          });
          const text = new THREE.Mesh(textGeometry, textMaterial);
          //   textGeometry.computeBoundingBox()
          //   textGeometry.translate(
          //     -(textGeometry.boundingBox.max.x - .02) * 0.5,
          //     -(textGeometry.boundingBox.max.y -.02) * 0.5,
          //     -(textGeometry.boundingBox.max.z - .03) * 0.5
          //   );
          textGeometry.center();
          scene.add(text);
        }
      );

      for (let i = 0; i < 100; i++) {
        const donutGeometry = new THREE.TorusGeometry(0.2, 0.2, 20, 45);
        const donutMaterial = new THREE.MeshMatcapMaterial({
          matcap: matcapTexture,
        });
        const donut = new THREE.Mesh(donutGeometry, material);
        donut.position.x = (Math.random() - 0.5) * 10;
        donut.position.y = (Math.random() - 0.5) * 10;
        donut.position.z = (Math.random() - 0.5) * 10;
        donut.rotation.x = Math.random() * Math.PI;
        donut.rotation.y = Math.random() * Math.PI;
        const scale = Math.random()
        donut.scale.set(scale, scale, scale)
        scene.add(donut);
      }
      /**
       * Lights
       */
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
      scene.add(ambientLight);

      const pointLight = new THREE.PointLight(0xffffff, 0.5);
      pointLight.position.x = 2;
      pointLight.position.y = 3;
      pointLight.position.z = 4;
      scene.add(pointLight);
      this.camera = new THREE.PerspectiveCamera(75, this.width / this.height);
      this.camera.position.x = 1;
      this.camera.position.z = 10;

      scene.add(this.camera);

      const controls = new OrbitControls(this.camera, canvas);
      controls.enableDamping = true;

      const renderer = new THREE.WebGLRenderer({
        canvas,
      });

      renderer.setSize(this.width, this.height);
      renderer.setPixelRatio(window.devicePixelRatio);

      const clock = new THREE.Clock();
      let that = this;
      function animate() {
        const elapsedTime = clock.getElapsedTime();
        sphere.rotation.y = 0.3 * elapsedTime;
        torus.rotation.y = 0.3 * elapsedTime;

        sphere.rotation.x = 0.15 * elapsedTime;
        torus.rotation.x = 0.15 * elapsedTime;

        controls.update();
        renderer.render(scene, that.camera);
        requestAnimationFrame(animate);
      }
      animate();
    },
  },
  mounted() {
    this.addScene();
  },
};
</script>

<style scoped>
</style>
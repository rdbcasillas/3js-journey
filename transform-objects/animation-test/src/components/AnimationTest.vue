<template>
  <div>
      <canvas class="webgl"></canvas>
  </div>
</template>

<script>
import * as THREE from 'three'
import gsap from 'gsap'
export default {
  data() {
      return {
          width: 800,
          height: 800,
      }
  },
  methods: {
    makeScene() {
      const scene = new THREE.Scene();
      const geometry = new THREE.BoxGeometry(2, 2, 2);
      const material = new THREE.MeshBasicMaterial({ color: "lightblue" });
      const mesh = new THREE.Mesh(geometry, material);
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

//      renderer.render(scene, camera);
      
      //Animation
      const clock = new THREE.Clock();
      
      function animate() {
          const elapsedTime = clock.getElapsedTime();
          mesh.rotation.x +=  .01* Math.cos(elapsedTime) 
          mesh.rotation.y += .01* Math.sin(elapsedTime)
          gsap.to(mesh.position, {duration: 1, delay: 1, x: Math.sin(elapsedTime)})
          gsap.to(mesh.position, {duration: 1, delay: 1, y: Math.cos(elapsedTime)})
          //mesh.rotation.z += .01*elapsedTime
          renderer.render(scene, camera);
          requestAnimationFrame(animate)
      }
      animate()
    },
  },
  mounted(){
      this.makeScene();
  }
};
</script>
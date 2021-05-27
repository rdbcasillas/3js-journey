<template>
  <div>
    <canvas class="webgl"></canvas>
  </div>
</template>

<script>
import * as THREE from "three";
import gsap from "gsap";
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

const scene = new THREE.Scene();
export default {
  data() {
    return {
      width: window.innerWidth,
      height: window.innerHeight,
      cursor: {
        x: 0,
        y: 0,
      },
      camera: {},
      renderObj: {},
      canv: {}
    };
  },
  methods: {
    addStar(){
        const stargeom = new THREE.SphereGeometry(0.25,24,24)
        const starmat = new THREE.MeshBasicMaterial({color: 'lightblue'})
        const star = new THREE.Mesh(stargeom, starmat)
        const [x, y, z] = Array(3).fill().map(() => THREE.MathUtils.randFloatSpread(100)); 
        star.position.set(x,y,z)
        scene.add(star)
    },
    makeScene() {
      const canvas = document.querySelector('.webgl')
      this.canv = canvas
      const geometry = new THREE.BoxGeometry(2, 2, 2);
      const material = new THREE.MeshBasicMaterial({ color: "green" });
      const mesh = new THREE.Mesh(geometry, material);

      scene.add(mesh);
      
      //Camera
      this.camera = new THREE.PerspectiveCamera(75, this.width / this.height);
      this.camera.position.x = 1;
      this.camera.position.z = 5;

      //orbit control for mouse interaction
      const controls = new OrbitControls(this.camera, canvas)
      controls.enableDamping = true
      scene.add(this.camera);

      //Renderer
      const renderer = new THREE.WebGLRenderer({
        canvas 
      });
      renderer.setSize(this.width, this.height);
      renderer.setPixelRatio(window.devicePixelRatio)
      this.renderObj = renderer;

      //Animation
      const clock = new THREE.Clock();
         
      let that = this
      function animate() {

        const elapsedTime = clock.getElapsedTime();
        //  mesh.rotation.x += 0.01 * Math.cos(elapsedTime);
        //  mesh.rotation.y += 0.01 * Math.sin(elapsedTime);
        // gsap.to(mesh.position, {
        //   duration: 1,
        //   delay: 1,
        //   x: Math.sin(elapsedTime),
        // });
        // gsap.to(mesh.position, {
        //   duration: 1,
        //   delay: 1,
        //   y: Math.cos(elapsedTime),
        // });

        //Update camera
        // camera.position.x = -(that.cursor.x * 10 )
        // camera.position.y = - (that.cursor.y * 10)
        //camera.lookAt(mesh.position)
        controls.update()
        renderer.render(scene, that.camera);
        requestAnimationFrame(animate);
      }
      animate();
    },
  },
  mounted() {
    window.addEventListener("mousemove", (event) => {
        this.cursor.x = event.clientX/800 - 0.5;
        this.cursor.y = event.clientY/800 - 0.5;
    });   
    window.addEventListener("resize", ()=>{
        this.width = window.innerWidth;
        this.height = window.innerHeight;

        this.camera.aspect = this.width/this.height;
        this.camera.updateProjectionMatrix();
        this.renderObj.setSize(this.width, this.height);

    })
    window.addEventListener('dblclick',()=>{
        if (!document.fullscreenElement) {
            console.log(this.canv)
            this.canv.requestFullscreen();
        }
        else {
            document.exitFullscreen();
        }
    })
    Array(300).fill().forEach(this.addStar);
    this.makeScene();
  },
};
</script>

<style scoped>
.webgl {
    position: fixed;
    top: 0;
    left: 0;
}
</style>
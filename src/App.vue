<template>
  <div>
    <h1>Pokemon</h1>
    <h3>{{ name }}</h3>
  </div>
  <canvas id="threeCanvas" width="300" height="200" class="center"></canvas>
  <img :src="spriteSrc" class="center" alt="" />

  <HelloWorld msg="Hello Vue 3 in CodeSandbox!" />
</template>

<script>
import axios from "axios";
import * as THREE from "three";

export default {
  name: "App",
  data() {
    return {
      info: null,
      data: null,
      name: "",
      spriteSrc: ""
    }
  },
  methods: {
    init: function() {
      const canvas = document.querySelector("#threeCanvas");

      const scene = new THREE.Scene();
      scene.background = new THREE.Color("#99e");
      const camera = new THREE.PerspectiveCamera( 75, canvas.width/canvas.height, 0.1, 1000 );
      camera.position.set(0, 0, 10);

      const geometry = new THREE.BoxGeometry( 1, 1, 1 );
      const material = new THREE.MeshBasicMaterial( { color: 0x00ffee } );
      const cube = new THREE.Mesh( geometry, material );
      scene.add( cube );

      const renderer = new THREE.WebGLRenderer({
        canvas,
        antialias: true,
      });
      renderer.setSize(canvas.clientWidth, canvas.clientHeight);


      const animate = function() {
        requestAnimationFrame(animate);
        renderer.render(scene, camera);
      }
      animate();
    }
  },
  mounted() {
    axios.get("https://pokeapi.co/api/v2/pokemon/eevee").then((response) => {
      this.info = response;
      this.data = this.info.data;

      const name = this.info.data.name;
      this.name = name.charAt(0).toUpperCase() + name.slice(1);
      this.spriteSrc = this.info.data.sprites.front_default;
    });

    this.init();
    //this.animate();
  }

}

</script>

<style scoped>
* {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 100%;
}
</style>
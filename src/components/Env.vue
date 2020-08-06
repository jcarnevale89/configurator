<template>
  <div>
    <input type="color" v-model="c" />
  </div>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";

export default {
  inject: ["$scene"],
  computed: {
    c: {
      get() {
        return `#${this.paintColor.getHexString()}`;
      },
      set(val) {
        this.paintColor.set(val);
      }
    }
  },
  methods: {
    update() {
      const mat = this.$scene.getMaterial("paint");
      mat.color = this.paintColor;
    }
  },
  created() {
    this.paintColor = new THREE.Color("#cc0000");

    this.$scene.setMaterial(
      "paint",
      new THREE.MeshPhongMaterial({
        color: this.paintColor
      })
    );
    // this.paintMaterial = new THREE.MeshPhongMaterial({
    //   color: this.paintColor
    // });
    this.groundMaterial = new THREE.MeshPhongMaterial({ color: "#999999" });

    const mesh = new THREE.Mesh(
      new THREE.PlaneBufferGeometry(100, 100),
      this.groundMaterial
    );
    mesh.rotateX(THREE.MathUtils.degToRad(-90));
    mesh.receiveShadow = true;
    this.$scene.add(mesh);

    var loader = new GLTFLoader();

    loader.load(
      "/lid.glb",
      gltf => {
        // console.log(gltf);
        gltf.scene.traverse(node => {
          if (node.isMesh) {
            node.castShadow = true;
            console.log(node);
            node.material = this.$scene.getMaterial("paint");
            // node.position.y = -1;
            // node.rotateZ(THREE.MathUtils.degToRad(45));
          }
        });
        this.$scene.add(gltf.scene);
      },
      undefined,
      function(error) {
        console.error(error);
      }
    );
  }
};
</script>

<style lang="stylus" scoped>
div
  position absolute
  top 10px
  left 0
  right 0
</style>

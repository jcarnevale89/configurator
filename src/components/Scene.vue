<template>
  <div>
    <div class="controls">
      <slot></slot>
    </div>
    <div ref="canvas"></div>
  </div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

export default {
  provide() {
    return {
      $scene: {
        add: this.addToScene,
        getMaterial: this.getMaterial,
        setMaterial: this.setMaterial
      }
    };
  },
  created() {
    this.scene = new THREE.Scene();
    this.scene.background = new THREE.Color(0xfafafa);

    this.camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );

    this.camera.position.x = 10;
    this.camera.position.y = 10;
    this.camera.position.z = 8;
    this.camera.rotateZ(THREE.MathUtils.degToRad(45));

    this.renderer = new THREE.WebGLRenderer();
    this.renderer.setSize(window.innerWidth, window.innerHeight);
    this.renderer.shadowMap.enabled = true;
    this.renderer.shadowMap.type = THREE.PCFShadowMap;

    this.controls = new OrbitControls(this.camera, this.renderer.domElement);
    this.controls.minDistance = 5;
    this.controls.maxDistance = 25;
    this.controls.enablePan = false;
    this.controls.maxPolarAngle = THREE.MathUtils.degToRad(85);

    this.materials = {
      default: new THREE.MeshBasicMaterial({ color: "#0194ca" })
    };

    const frontLight = new THREE.PointLight(0xffffff, 1.25, 100);
    frontLight.position.x = 5;
    frontLight.position.y = 5;
    frontLight.position.z = 6;
    // frontLight.castShadow = true;
    frontLight.add(
      new THREE.Mesh(new THREE.SphereBufferGeometry(), this.materials.default)
    );
    this.scene.add(frontLight);

    const backLight = new THREE.PointLight(0xffffff, 1.25, 100);
    backLight.position.x = -5;
    backLight.position.y = 5;
    backLight.position.z = -6;
    // backLight.castShadow = true;
    backLight.add(
      new THREE.Mesh(new THREE.BoxBufferGeometry(), this.materials.default)
    );
    this.scene.add(backLight);
  },
  mounted() {
    this.$refs.canvas.appendChild(this.renderer.domElement);

    this.animate();
  },
  methods: {
    addToScene(obj) {
      this.scene.add(obj);
    },
    getMaterial(key) {
      return this.materials[key];
    },
    setMaterial(key, material) {
      this.materials[key] = material;
    },
    animate() {
      requestAnimationFrame(this.animate);
      this.$children.forEach(c => {
        if (c.update) c.update();
      });
      this.renderer.render(this.scene, this.camera);
      this.controls.update();
    }
  }
};
</script>

<style lang="stylus" scoped>
.controls
  position relative
  height 100%
  width 100%
</style>

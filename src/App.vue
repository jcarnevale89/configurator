<template>
  <div id="app">
    <Scene>
      <Env />
    </Scene>
  </div>
</template>

<script>
import Scene from "./components/Scene";
import Env from "./components/Env";

export default {
  name: "App",
  components: {
    Scene,
    Env
  }
};
</script>

<style lang="stylus">
body
#app
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  padding 0
  margin 0
</style>

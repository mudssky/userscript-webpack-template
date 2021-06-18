<template>
  <div id="test">{{ msg }}</div>
  <!-- <div>{{console.log(process)}}</div> -->
</template>

<script lang="ts">
export default {
  data() {
    return {
      msg: 'hello vue3',
    }
  },
}
</script>

<style lang="scss">
#app {
  color: red;
  background-color: yellow;
}
</style>

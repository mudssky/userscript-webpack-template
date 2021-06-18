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
#test {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 999999999999999999;
  color: red;
  background-color: yellow;
}
</style>

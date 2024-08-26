<template>
  <div id="app">
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
    <WaterMask />
  </div>
</template>

<script>
import WaterMask from './components/WaterMask.vue';

export default {
  name: 'App',
  components: {
    WaterMask,
  },
  data() {
    return {
    };
  },
};
</script>

<style lang="less">
</style>


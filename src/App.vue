<template>
  <div style="height:100%">
    <router-view></router-view>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  created() {
    this.$router.push('/login');
  }
}
</script>

<style>
	@import './style/common';
</style>
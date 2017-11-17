<template>
  <div id="app">
    <encrypt></encrypt>
  </div>
</template>

<script>
  import LandingPage from '@/components/LandingPage'
  import encrypt from '@/components/encrypt';

  export default {
    name: 'electron-des-ecb',
    components: {
      encrypt
    }
  }
</script>

<style>
  /* CSS */
</style>

<template>
  <div id="app">
    <!-- <headerbar></headerbar> -->
    <encrypt></encrypt>
  </div>
</template>

<script>
  import LandingPage from '@/components/LandingPage'
  import encrypt from '@/components/encrypt';
  import headerbar from "@/components/headerbar";
  export default {
    name: 'electron-des-ecb',
    components: {
      encrypt,
      headerbar
    }
  }
</script>

<style>
  /* CSS */
</style>

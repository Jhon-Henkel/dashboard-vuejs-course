<template>
  <Navbar :logo="logoSrc" :alt="altSrc"/>
  <router-view/>
  <Footer/>
</template>

<style>

</style>
<script>
  import Navbar from "@/components/Navbar.vue";
  import Footer from "@/components/Footer.vue";

  export default {
    components: {
      Navbar,
      Footer
    },
    data() {
      return {
        logoSrc: '/img/logo.png',
        altSrc: 'Make your burger'
      }
    }
  }
</script>
<style>
  * {
    font-family: "Helvetica", serif;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  .main-container {
    margin: 50px;
    min-height: 602px;
  }

  h1 {
    text-align: center;
    font-size: 42px;
    margin-bottom: 30px;
    color: #222;
  }
</style>
<template>
  <Banner/>
  <div class="main-container">
    <h1>Monte seu burger:</h1>
    <p>FORMULARIO</p>
  </div>
</template>

<script>
  import {defineComponent} from "vue";
  import Banner from "@/components/Banner.vue";

  export default defineComponent({
    name: 'Home',
    components: {
      Banner,
    }
  })
</script>
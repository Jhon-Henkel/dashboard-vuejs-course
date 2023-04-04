<template>
  <Banner/>
  <div class="main-container">
    <h1>Monte seu burger:</h1>
    <BurgerForm/>
  </div>
</template>

<script>
  import {defineComponent} from "vue";
  import Banner from "@/components/Banner.vue";
  import BurgerForm from "@/components/BurgerForm.vue";

  export default defineComponent({
    name: 'Home',
    components: {
      BurgerForm,
      Banner,
    }
  })
</script>
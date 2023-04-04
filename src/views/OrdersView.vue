<template>
  <div class="main-container">
    <h1>Gerenciar Pedidos</h1>
    <OrdersDashboard/>
  </div>
</template>

<script>
  import OrdersDashboard from "@/components/OrdersDashboard.vue";

  export default {
    name: 'ordersView',
    components: {
      OrdersDashboard
    }
  }
</script>
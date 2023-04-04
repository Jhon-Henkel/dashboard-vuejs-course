<template>
  <div id="burger-table">
    <Message :message="message" v-show="message"/>
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="order in burgers" :key="order.id">
        <div class="order-number">{{ order.id }}</div>
        <div>{{ order.nome }}</div>
        <div>{{ order.pao }}</div>
        <div>{{ order.carne }}</div>
        <div>
          <ul>
            <li v-for="(opcional, index) in order.opcionais" :key="index">
              {{ opcional }}
            </li>
          </ul>
        </div>
        <div>
          <select name="staus" class="staus" @change="updateStatus($event, order.id)">
            <option
                v-for="thisStatus in status"
                :key="thisStatus.id"
                :value="thisStatus.tipo"
                :selected="order.status === thisStatus.tipo">
              {{ thisStatus.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteOrder(order.id)">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Message from "@/components/Message.vue";

  export default {
    name: "OrdersDashboard",
    components: {
      Message
    },
    data() {
      return {
        burgers: null,
        burgerId: null,
        status: [],
        message: null
      }
    },
    methods: {
      async getOrders() {
        const request = await fetch('http://localhost:3000/burgers')
        this.burgers = await request.json()
      },
      async getStatus() {
        const request = await fetch('http://localhost:3000/status')
        this.status = await request.json()
      },
      async deleteOrder(orderId) {
        const request = await fetch(`http://localhost:3000/burgers/${orderId}`, {method: 'DELETE'})
        const response = request.json()
        this.getOrders()
        this.message = `Pedido ${orderId} deletado com sucesso!`
        setTimeout(() => this.message = null, 5000)
      },
      async updateStatus(event, orderId) {
        const option = event.target.value
        const dataJson = JSON.stringify({status: option})
        const request = await fetch(
            `http://localhost:3000/burgers/${orderId}`,
            {method: 'PATCH', headers: {'Content-Type': "application/json"}, body: dataJson}
        )
        const response = await request.json()
        this.message = `Pedido ${orderId} atualizado com sucesso para ${response.status}!`
        setTimeout(() => this.message = null, 5000)
      }
    },
    mounted() {
      this.getOrders()
      this.getStatus()
    }
  }
</script>

<style scoped>
  #burger-table {
    max-width: 1200px;
    margin: 0 auto;
  }

  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row {
    display: flex;
    flex-wrap: wrap;
  }

  #burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }

  #burger-table-heading div,
  .burger-table-row div {
    width: 19%;
  }

  .burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 5%;
  }

  select {
    padding: 12px 6px;
    margin-right: 12px;
  }

  .delete-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }

  .delete-btn :hover {
    background-color: transparent;
    color: #222;
  }
</style>
<template>
  <div>
    <Message :message="message" v-show="message"/>
    <div>
      <form id="burger-form" @submit="createBurger">
        <div class="input-container">
          <label for="client-name">Nome do Cliente:</label>
          <input type="text" id="client-name" v-model="clientName" placeholder="Digite o seu nome">
        </div>
        <div class="input-container">
          <label for="bread">Escolha o pão:</label>
          <select name="bread" id="bread" v-model="bread">
            <option value="">Selecione o pão</option>
            <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">{{ bread.tipo }}</option>
          </select>
        </div>
        <div class="input-container">
          <label for="meat">Escolha a carne do seu burger:</label>
          <select name="meat" id="meat" v-model="meat">
            <option value="">Selecione o tipo de carne</option>
            <option v-for="meat in meats" :key="meat.id" :value="meat.tipo">{{ meat.tipo }}</option>
          </select>
        </div>
        <div class="input-container" id="options-container">
          <label for="options" id="options-title">Selecione os opicionais:</label>
          <div class="checkbox-container" v-for="option in options" :key="option.id">
            <input type="checkbox" name="options" v-model="optionSelected" :value="option.tipo">
            <span>{{ option.tipo }}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Make my burger!">
        </div>
      </form>
    </div>
  </div>
</template>

<script>
  import Message from "@/components/Message.vue";

  export default {
    name: "BurgerForm",
    components: {
      Message
    },
    data() {
      return {
        clientName: null,
        breads: null,
        meats: null,
        options: null,
        bread: null,
        meat: null,
        optionSelected: [],
        message: null
      }
    },
    methods: {
      async getIngredients() {
        const request = await fetch('http://localhost:3000/ingredientes')
        const data = await request.json()
        this.breads = data.paes
        this.meats = data.carnes
        this.options = data.opcionais
      },
      async createBurger(event) {
        event.preventDefault()
        const data = {
          nome: this.clientName,
          carne: this.meat,
          pao: this.bread,
          opcionais: Array.from(this.optionSelected),
          status: 'Solicitado'
        }
        const dataJson = JSON.stringify(data)
        const request = await fetch('http://localhost:3000/burgers', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: dataJson
        })
        const response = await request.json()
        this.message = `Pedido Nº ${response.id} realizado com sucesso!`
        setTimeout(() => this.message = null, 5000)
        this.clearFormData()
      },
      clearFormData() {
        this.clientName = null
        this.bread = null
        this.meat = null
        this.optionSelected = []
      }
    },
    mounted() {
      this.getIngredients()
    }
  }
</script>

<style scoped>
  #burger-form {
    max-width: 400px;
    margin: 0 auto;
  }

  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }

  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222222;
    padding: 5px 10px;
    border-left: 4px solid #FCBA03;
  }

  input, select {
    padding: 5px 10px;
    width: 300px;
  }

  #options-container {
    flex-direction: row;
    flex-wrap: wrap;
  }

  #options-title {
    width: 100%;
  }

  .checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
  }

  .checkbox-container span,
  .checkbox-container input {
    width: auto;
  }

  .checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
  }

  .submit-btn {
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

  .submit-btn :hover {
    background-color: transparent;
    color: #222;
  }
</style>
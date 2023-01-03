<template>
  <div>
    <Message :msg="msg" v-show="msg" />
  </div>
  <form id="burger-form" @submit="createBurger">
    <div class="input-container">
      <label for="nome">nome do cliente:</label>
      <input
        type="text"
        id="nome"
        name="nome"
        v-model="nome"
        placeholder="Digite o seu nome"
      />
    </div>

    <Pedra :data="paes" :text="'Escolha seu pão'" />
    <Pedra :data="carnes" :text="'Escolha sua carne'" />

    <div class="input-container">
      <label for="opcionais">Selecione os opcionais:</label>
      <div
        class="checkbox-container"
        v-for="opcional in opcionaisdata"
        :key="opcional.id"
      >
        <input
          type="checkbox"
          name="opcionais"
          v-model="opcionais"
          :value="opcional.tipo"
        />
        <span>{{ opcional.tipo }}</span>
      </div>
    </div>
    <div class="input-container">
      <input type="submit" class="submit-btn" value="Criar meu Burger" />
    </div>
  </form>
</template>

<script>
import Message from "./Message.vue";
import Pedra from "./Pedra.vue";
export default {
  name: "BurgerForm",

  data() {
    return {
      paes: null,
      carnes: null,
      opcionaisdata: null,
      nome: null,
      pao: null,
      value: null,
      carne: null,
      opcionais: [],
      msg: null,
    };
  },
  methods: {
    async getIngredientes() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();
      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },
    async createBurger(e) {
      e.preventDefault();

      const data = {
        nome: this.nome,
        carne: this.value,
        pao: this.value,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/burgers", {
        method: "Post",
        headers: { "content-type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      this.msg = `Pedido n° ${res.id} realizado com sucesso`;
      setTimeout(() => (this.msg = ""), 3000);

      this.nome = "";
      this.carne = "";
      this.opcionais = "";
      this.pao = "";
    },
  },
  mounted() {
    this.getIngredientes();
  },
  components: {
    Message,
    Pedra,
  },
};
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
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}
input,
select {
  padding: 5px 10px;
  width: 300px;
}
#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}
#opcionais-title {
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
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}
.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>

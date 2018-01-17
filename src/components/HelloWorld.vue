<template>
  <div class="hello">
    <div class="alert alert-primary" role="alert">
      {{msg}}
    </div>
    <!-- <h1>{{ msg }}</h1> -->
    <h2>Insira a porta a ser aberta e acione o botão</h2>
    <form @submit.prevent="testar">
      <!-- <input type="number" v-model="porta" placeholder="insira a porta">  -->
        <div class="form-group">
          <!-- <label for="exampleInputPassword1">Insira a porta</label> -->
          <input type="number" class="form-control" v-model="porta" placeholder="insira a porta">
        </div>
        <button type="submit" class="button btn-primary ">Testar</button>
      <!-- <button v-on:click="testar">Testar Porta</button> -->
    </form>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";

Vue.use(VueAxios, axios);
porta: 0;
export default {
  name: "HelloWorld",
  data() {
    return {
      porta: 0,
      msg: "Ambiente de Teste - ProtectM."
    };
  },
  methods: {
    testar() {
      console.log("porta: 1" + this.porta);
      //Chamar api do dataplicity para abrir a porta

      // Requisições POST, note há um parâmetro extra indicando os parâmetros da requisição
      axios
        .post(
          "https://uncurtained-bonobo-2698.dataplicity.io/led/" +
            this.porta +
            "/",
          {
            state: "0"
          }
        )
        .then(function(response) {
          console.log("opened");
          console.log(Object.keys(response.data));
          axios
            .post(
              "https://uncurtained-bonobo-2698.dataplicity.io/led/" +
                Object.keys(response.data) +
                "/",
              {
                state: "1"
              }
            )
            .then(function(response) {
              console.log("closed");
            });
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

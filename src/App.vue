
<template>
  <div id="app">
    <nav>
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="brand-logo center">Produtos Front</a>
      </div>
    </nav>
    <div class="container">
      <form @submit.prevent="salvar">
        <label>Nome</label>
        <input type="text" placeholder="Nome" v-model="produto.nome">
        <label>Quantidade</label>
        <input type="number" placeholder="QTD" v-model="produto.quantidade">
        <label>Valor</label>
        <input type="text" placeholder="Valor" v-model="produto.valor">

        <button class="waves-effect waves-light btn-small">
          Salvar
          <i class="material-icons left">save</i>
        </button>
      </form>

      <table>
        <thead>
          <tr>
            <th>NOME</th>
            <th>QTD</th>
            <th>VALOR</th>
            <th>OPÇÕES</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="cd of produtos" :key="cd.id">
            <td>{{ cd.nome }}</td>
            <td>{{ cd.quantidade }}</td>
            <td>{{ cd.valor }}</td>
            <td>
              <button @click="editar(cd)" class="waves-effect btn-small blue darken-1">
                <i class="material-icons">create</i>
              </button>
              <button @click="remover(cd)" class="waves-effect btn-small red darken-1">
                <i class="material-icons">delete_sweep</i>
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import Produto from "./services/produtos";

export default {
  data() {
    return {
      produtos: [],
      produto: {
        nome: "",
        quantidade: "",
        valor: ""
      }
    };
  },

  mounted(){
    this.listar();
  },

  methods: {
    listar() {
      Produto.listar().then(resposta => {
        console.log(resposta.data);
        this.produtos = resposta.data;
      });
    },

    salvar(){
      if (!this.produto.id) {
        Produto.salvar(this.produto).then(resposta => {
          this.produto = {};
          alert("Salvo com sucesso!");
          this.listar();
        });
      } else {
        Produto.atualizar(this.produto).then(resposta => {
          this.produto = {};
          alert("Atualizado com sucesso!");
          this.listar();
        });
      }
    },

    editar(produto){
      this.produto = produto;
    },

    remover(produto){
      Produto.apagar(produto).then(resposta =>{
        this.listar();
      })
    }
  }
};
</script>

<style>
</style>

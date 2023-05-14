<script>
import AutoresApi from "@/api/autores";
const autoresApi = new AutoresApi();
export default {
  data() {
    return {
      autores: [],
      autor: {},
      itemSelecionado: null,
    };
  },
  async created() {
    this.autores = await autoresApi.buscarTodosOsAutores();
  },

  watch: {
    itemSelecionado(novoValor) {
      if (novoValor != null) {
        document.getElementById("divBody").classList.add("blur");
        document.getElementById("divNav").classList.add("blur");
      } else {
        document.getElementById("divBody").classList.remove("blur");
        document.getElementById("divNav").classList.remove("blur");
      }
    },
  },

  methods: {
    async selecionarItem(autor) {
      this.itemSelecionado = autor;
    },

    async salvar() {
      if (this.autor.id) {
        await autoresApi.atualizarAutor(this.autor);
      } else {
        await autoresApi.adicionarAutor(this.autor);
      }
      this.autor = {};
      this.autores = await autoresApi.buscarTodosOsAutores();
    },
    editar(autor) {
      Object.assign(this.autor, autor);
    },
    async excluir(autor) {
      await autoresApi.excluirAutor(autor.id);
      this.autores = await autoresApi.buscarTodosOsAutores();
    },
  },
};
</script>

<template>
  <div class="painelDetalhes" v-if="itemSelecionado">
    <span class="btnFechar" @click="itemSelecionado = null">X</span>
    <div>ID: {{ itemSelecionado.id }}</div>
    <div>Nome: {{ itemSelecionado.nome }}</div>
    <div>Email: {{ itemSelecionado.email }}</div>
  </div>
  <div id="divBody">
    <h1>Autor</h1>
    <hr />
    <div class="form">
      <input
        class="inputAdd"
        type="text"
        v-model="autor.nome"
        placeholder="Nome"
      />
      <input
        class="inputAdd"
        type="text"
        v-model="autor.email"
        placeholder="Email"
      />
      <button class="salvarBtn" @click="salvar">Salvar</button>
    </div>
    <div>
      <hr />
      <ul>
        <li class="listaCategoria" v-for="autor in autores" :key="autor.id">
          <span class="itemCategoria" @click="selecionarItem(autor)">
            <div class="divNome">{{ autor.nome }}</div>
          </span>
          <span class="editarBtn" @click="editar(autor)">Editar</span>
          <span class="deleteBtn" @click="excluir(autor)">X</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<style></style>

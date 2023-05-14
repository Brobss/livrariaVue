<script>
import CategoriasApi from "@/api/categorias";
const categoriasApi = new CategoriasApi();
export default {
  data() {
    return {
      categorias: [],
      categoria: {},
      itemSelecionado: null,
    };
  },
  async created() {
    this.categorias = await categoriasApi.buscarTodasAsCategorias();
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
    async selecionarItem(categoria) {
      this.itemSelecionado = categoria;
    },

    async salvar() {
      if (this.categoria.id) {
        await categoriasApi.atualizarCategoria(this.categoria);
      } else {
        await categoriasApi.adicionarCategoria(this.categoria);
      }
      this.categoria = {};
      this.categorias = await categoriasApi.buscarTodasAsCategorias();
    },
    editar(categoria) {
      Object.assign(this.categoria, categoria);
    },
    async excluir(categoria) {
      await categoriasApi.excluirCategoria(categoria.id);
      this.categorias = await categoriasApi.buscarTodasAsCategorias();
    },
  },
};
</script>

<template>
  <div class="painelDetalhes" v-if="itemSelecionado">
    <span class="btnFechar" @click="itemSelecionado = null">X</span>
    <div>ID: {{ itemSelecionado.id }}</div>
    <div>Descrição: {{ itemSelecionado.descricao }}</div>
  </div>
  <div id="divBody">
    <h1>Categoria</h1>
    <hr />
    <div class="form">
      <input
        class="inputAdd"
        type="text"
        v-model="categoria.descricao"
        placeholder="Descrição"
      />
      <button class="salvarBtn" @click="salvar">Salvar</button>
    </div>
    <hr />
    <ul>
      <li
        class="listaCategoria"
        v-for="categoria in categorias"
        :key="categoria.id"
      >
        <span class="itemCategoria" @click="selecionarItem(categoria)">
          {{ categoria.descricao }}
        </span>
        <span class="deleteBtn" @click="excluir(categoria)">X</span>
      </li>
    </ul>
  </div>
</template>

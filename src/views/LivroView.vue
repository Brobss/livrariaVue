<script>
import LivrosApi from "@/api/livros";
const livrosApi = new LivrosApi();
import AutoresApi from "@/api/autores";
const autoresApi = new AutoresApi();
import EditorasApi from "@/api/editoras";
const editorasApi = new EditorasApi();
import CategoriasApi from "@/api/categorias";
const categoriasApi = new CategoriasApi();
export default {
  data() {
    return {
      livros: [],
      livro: {},
      autor: {},
      editora: {},
      categoria: {},
      itemSelecionado: null,
    };
  },
  async created() {
    this.livros = await livrosApi.buscarTodosOsLivros();
    this.autores = await autoresApi.buscarTodosOsAutores();
    this.editoras = await editorasApi.buscarTodasAsEditoras();
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
    async selecionarItem(livro) {
      this.itemSelecionado = livro;
    },

    async salvar() {
      if (this.livro.id) {
        await livrosApi.atualizarLivro(this.livro);
      } else {
        await livrosApi.adicionarLivro(this.livro);
      }
      this.livro = {};
      this.livros = await livrosApi.buscarTodosOsLivros();
    },
    editar(livro) {
      Object.assign(this.livro, livro);
    },
    async excluir(livro) {
      await livrosApi.excluirLivro(livro.id);
      this.livros = await livrosApi.buscarTodosOsLivros();
    },
  },
};
</script>

<template>
  <div class="painelDetalhes" v-if="itemSelecionado">
    <span class="btnFechar" @click="itemSelecionado = null">X</span>
    <div>ID: {{ itemSelecionado.id }}</div>
    <div>Título: {{ itemSelecionado.titulo }}</div>
    <div>ISBN: {{ itemSelecionado.isbn }}</div>
    <div>Quantidade: {{ itemSelecionado.quantidade }}</div>
    <div>Preço: R${{ itemSelecionado.preco }}</div>
    <div>Categoria: {{ itemSelecionado.categoria.descricao }}</div>
    <div>Autor: {{ itemSelecionado.autor.nome }}</div>
    <div>Editora: {{ itemSelecionado.editora.nome }}</div>
  </div>
  <div id="divBody">
    <h1>Livro</h1>
    <hr />
    <div class="form">
      <input
        class="inputAdd"
        type="text"
        v-model="livro.titulo"
        placeholder="Nome"
      />
      <input
        class="inputAdd"
        type="text"
        v-model="livro.isbn"
        placeholder="ISBN"
      />
      <input
        class="inputAdd"
        type="text"
        v-model="livro.quantidade"
        placeholder="Quantidade"
      />
      <input
        class="inputAdd"
        type="text"
        v-model="livro.preco"
        placeholder="Preço"
      />
      <select class="selectDropdown" v-model="livro.autor" id="">
        <option v-for="autor in autores" :key="autor.id" :value="autor.id">
          {{ autor.nome }}
        </option>
      </select>
      <select class="selectDropdown" v-model="livro.editora" id="">
        <option
          v-for="editora in editoras"
          :key="editora.id"
          :value="editora.id"
        >
          {{ editora.nome }}
        </option>
      </select>
      <select class="selectDropdown" v-model="livro.categoria" id="">
        <option
          v-for="categoria in categorias"
          :key="categoria.id"
          :value="categoria.id"
        >
          {{ categoria.descricao }}
        </option>
      </select>
      <button class="salvarBtn" @click="salvar">Salvar</button>
    </div>
    <hr />
    <ul>
      <li class="listaCategoria" v-for="livro in livros" :key="livro.id">
        <span class="itemCategoria" @click="selecionarItem(livro)">
          <!--<div class="divID">{{ livro.id }}</div> -->
          <div class="divNome">{{ livro.titulo }}</div>
          <!--<div class="divSite"><u>Site:</u> {{ livro.site }}</div>-->
        </span>
        <span class="editarBtn" @click="editar(livro)">Editar</span>
        <span class="deleteBtn" @click="excluir(livro)">X</span>
      </li>
    </ul>
  </div>
</template>

<style></style>

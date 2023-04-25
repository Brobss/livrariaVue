<script>
import LivrosApi from "@/api/livros";
const livrosApi = new LivrosApi();
export default {
  data() {
    return {
      livros: [],
      livro: {},
      livroSelecionado: null,
    };
  },
  async created() {
    this.livros = await livrosApi.buscarTodosOsLivros();
  },
  watch: {
    livroSelecionado(novoValor) {
      if (novoValor != null) {
        document.getElementById("divBody").classList.add("blur");
      } else {
        document.getElementById("divBody").classList.remove("blur");
      }
    },
  },
  methods: {
    async selecionarLivro(livro) {
      this.livroSelecionado = livro;
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
  <div class="painelDetalhes" v-if="livroSelecionado">
    <span class="btnFechar" @click="livroSelecionado = null">X</span>
    <div>ID: {{ livroSelecionado.id }}</div>
    <div>Título: {{ livroSelecionado.titulo }}</div>
    <div>ISBN: {{ livroSelecionado.isbn }}</div>
    <div>Quantidade: {{ livroSelecionado.quantidade }}</div>
    <div>Preço: {{ livroSelecionado.preco }}</div>
    <div>Categoria: {{ livroSelecionado.categoria.descricao }}</div>
    <div>Autor: {{ livroSelecionado.autor.nome }}</div>
    <div>Editora: {{ livroSelecionado.editora.nome }}</div>
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
        placeholder="Site"
      />
      <input
        class="inputAdd"
        type="text"
        v-model="livro.quantidade"
        placeholder="Site"
      />
      <input
        class="inputAdd"
        type="text"
        v-model="livro.preco"
        placeholder="Site"
      />
      <button class="salvarBtn" @click="salvar">Salvar</button>
    </div>
    <hr />
    <ul>
      <li class="listaCategoria" v-for="livro in livros" :key="livro.id">
        <span class="itemCategoria" @click="selecionarLivro(livro)">
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

<style>
.listaCategoria {
  justify-content: space-between;
  margin: 10px;
  border: 1px solid black;
  border-radius: 12px;
  transition: 0.5s;
}

.listaCategoria:hover {
  background-color: darkgray;
  cursor: pointer;
}

.itemCategoria {
  font-size: larger;
  color: black;
}

.deleteBtn {
  padding: 5px;
  margin: 5px;
  font-size: larger;
  color: red;
  transition: 0.2s;
}

.deleteBtn:hover {
  color: darkred;
  cursor: pointer;
  transform: scale(1.4);
}

.inputAdd {
  padding: 10px;
  margin: 10px;
  border: 1px solid black;
  border-radius: 12px;
  font-size: larger;
}

.salvarBtn {
  padding: 10px;
  margin: 10px;
  border: 1px solid black;
  border-radius: 12px;
  font-size: larger;
  transition: 0.2s;
}

.salvarBtn:hover {
  background-color: darkgray;
  cursor: pointer;
  transform: scale(1.2);
}

.divID {
  display: block;
  width: 50px;
  font-weight: bolder;
  font-size: larger;
}

.divNome {
  display: block;
  width: 300px;
  font-style: italic;
}

.divSite {
  display: flex;
  flex-direction: row;
  width: 200px;
}

.painelDetalhes {
  position: fixed;
  margin: 10px;
  padding: 10px;
  border: 1px solid black;
  border-radius: 12px;
  font-size: larger;
  background-color: grey;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.btnFechar {
  position: absolute;
  top: 0;
  right: 0;
  padding: 8px;
  margin: -8px;
  font-size: larger;
  color: red;
  transition: 0.2s;
  width: 30px;
  height: 30px;
  display: block;
}

.btnFechar:hover {
  color: darkred;
  cursor: pointer;
  transform: scale(1.4);
}

.blur {
  transition: 1s;
  filter: blur(2px);
  pointer-events: none;
}
</style>

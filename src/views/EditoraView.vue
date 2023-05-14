<script>
import EditorasApi from "@/api/editoras";
const editorasApi = new EditorasApi();
export default {
  data() {
    return {
      editoras: [],
      editora: {},
      itemSelecionado: null,
    };
  },
  async created() {
    this.editoras = await editorasApi.buscarTodasAsEditoras();
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
    async selecionarItem(editora) {
      this.itemSelecionado = editora;
    },

    async salvar() {
      if (this.editora.id) {
        await editorasApi.atualizarEditora(this.editora);
      } else {
        await editorasApi.adicionarEditora(this.editora);
      }
      this.editora = {};
      this.editoras = await editorasApi.buscarTodasAsEditoras();
    },
    editar(editora) {
      Object.assign(this.editora, editora);
    },
    async excluir(editora) {
      await editorasApi.excluirEditora(editora.id);
      this.editoras = await editorasApi.buscarTodasAsEditoras();
    },
  },
};
</script>

<template>
  <div class="painelDetalhes" v-if="itemSelecionado">
    <span class="btnFechar" @click="itemSelecionado = null">X</span>
    <div>ID: {{ itemSelecionado.id }}</div>
    <div>Nome: {{ itemSelecionado.nome }}</div>
    <div>Site: {{ itemSelecionado.site }}</div>
  </div>
  <div id="divBody">
    <h1>Editora</h1>
    <hr />
    <div class="form">
      <input
        class="inputAdd"
        type="text"
        v-model="editora.nome"
        placeholder="Nome"
      />
      <input
        class="inputAdd"
        type="text"
        v-model="editora.site"
        placeholder="Site"
      />
      <button class="salvarBtn" @click="salvar">Salvar</button>
    </div>
    <hr />
    <ul>
      <li class="listaCategoria" v-for="editora in editoras" :key="editora.id">
        <span class="itemCategoria" @click="selecionarItem(editora)">
          <!--<div class="divID">{{ editora.id }}</div> -->
          <div class="divNome">{{ editora.nome }}</div>
          <!--<div class="divSite"><u>Site:</u> {{ editora.site }}</div>-->
        </span>
        <span class="editarBtn" @click="editar(editora)">Editar</span>
        <span class="deleteBtn" @click="excluir(editora)">X</span>
      </li>
    </ul>
  </div>
</template>

<style></style>

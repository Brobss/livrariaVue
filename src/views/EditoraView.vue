<script>
import EditorasApi from "@/api/editoras";
const editorasApi = new EditorasApi();
export default {
  data() {
    return {
      editoras: [],
      editora: {},
      editoraSelecionada: null,
    };
  },
  async created() {
    this.editoras = await editorasApi.buscarTodasAsEditoras();
  },
  watch: {
    editoraSelecionada(novoValor) {
      if (novoValor != null) {
        document.getElementById("divBody").classList.add("blur");
      } else {
        document.getElementById("divBody").classList.remove("blur");
      }
    },
  },
  methods: {
    async selecionarEditora(editora) {
      this.editoraSelecionada = editora;
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
  <div class="painelDetalhes" v-if="editoraSelecionada">
    <span class="btnFechar" @click="editoraSelecionada = null">X</span>
    <div>ID: {{ editoraSelecionada.id }}</div>
    <div>Nome: {{ editoraSelecionada.nome }}</div>
    <div>Site: {{ editoraSelecionada.site }}</div>
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
        <span class="itemCategoria" @click="selecionarEditora(editora)">
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

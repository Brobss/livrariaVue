<script>
import AutoresApi from "@/api/autores";
const autoresApi = new AutoresApi();
export default {
  data() {
    return {
      autores: [],
      autor: {},
    };
  },
  async created() {
    this.autores = await autoresApi.buscarTodosOsAutores();
  },
  methods: {
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
  <hr />
  <ul>
    <li class="listaCategoria" v-for="autor in autores" :key="autor.id">
      <span class="itemCategoria" @click="editar(autor)">
        <div class="divID">{{ autor.id }}</div>
        <div class="divNome">{{ autor.nome }}</div>
        <div class="divEmail"><u>Email:</u>{{ autor.email }}</div>
      </span>
      <span class="deleteBtn" @click="excluir(autor)">X</span>
    </li>
  </ul>
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

.divID {
  display: block;
  width: 50px;
  font-weight: bolder;
  font-size: larger;
}

.divNome {
  display: block;
  width: 200px;
  font-style: italic;
}

.divEmail {
  display: flex;
  flex-direction: row;
  width: 200px;
}
</style>

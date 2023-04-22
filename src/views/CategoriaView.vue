<script>
import CategoriasApi from "@/api/categorias";
const categoriasApi = new CategoriasApi();
export default {
  data() {
    return {
      categorias: [],
      categoria: {},
    };
  },
  async created() {
    this.categorias = await categoriasApi.buscarTodasAsCategorias();
  },
  methods: {
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
      <span class="itemCategoria" @click="editar(categoria)">
        {{ categoria.id }} - {{ categoria.descricao }}
      </span>
      <span class="deleteBtn" @click="excluir(categoria)">X</span>
    </li>
  </ul>
</template>

<style>
.listaCategoria {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
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
  padding: auto;
  margin: auto;
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
</style>

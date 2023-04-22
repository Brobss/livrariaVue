<template>
  <div class="divHeader">
    <router-link class="itemHeader" :to="{ name: 'home' }">Home</router-link>
    <router-link class="itemHeader" to="/categorias">Categorias</router-link>
    <router-link class="itemHeader" to="/editoras">Editoras</router-link>
    <router-link class="itemHeader" to="/autores">Autores</router-link>
    <router-link class="itemHeader" to="/livros">Livros</router-link>
  </div>
  <main>
    <RouterView />
  </main>
</template>

<style>
.divHeader {
  align-items: center;
  padding: 25px;
  border: 2px ridge black;
}

.itemHeader {
  padding: 10px;
  font-size: larger;
  border-radius: 12px;
}

.itemHeader:hover {
  border: 2px solid green;
  background-color: lightgreen;
}
</style>

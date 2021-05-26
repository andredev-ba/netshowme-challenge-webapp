<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">
        challenge
      </h1>
      <div class="links">
        <NuxtLink tag="a" class="button--grey" to="/contacts/form-page">Formulário de Contato</NuxtLink>
        <NuxtLink tag="a" class="button--grey" to="/contacts/list">Lista de Contatos</NuxtLink>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({})
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 70x;
  color: #35495e;
  letter-spacing: 2px;
  margin-top: 5px;
}

.links {
  padding-top:40px;
}

.links a {
  width: 250px;
}
</style>

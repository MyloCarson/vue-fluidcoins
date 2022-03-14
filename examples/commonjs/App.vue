<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import Fluidcoins from '../../lib/fluidcoins.vue'

const onSuccess = function (payload) {
      console.log(payload)
}
</script>

<template>
  <fluidcoins 
    type="secondary"
    rounded
    publicKey="pk_test_73075981bc654fb48d38d8de8aaf82f4"
    :amount="90000"
    email="seun@fluidcoins.com"
    :on-success="onSuccess"
    />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

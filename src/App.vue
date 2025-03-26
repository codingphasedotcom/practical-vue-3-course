<script setup>
import { ref } from 'vue';

const name = "Joe";
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import RegisterForm from './components/RegisterForm.vue'


</script>

<template>
  <Header />
  <RegisterForm />
  <Footer />
  
</template>

<style scoped>
  .popup{
    background: blue;
    height: 200px;
  }
  .username{
    font-size: 4rem;
    color: red;
  }
</style>

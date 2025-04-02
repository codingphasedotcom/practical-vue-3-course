<script setup>
import { ref } from 'vue';

const name = "Joe";
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import Pokedex from './completed/Pokedex.vue'
import RegisterForm from './completed/RegisterForm.vue'
import CarLoan from './components/CarLoan.vue'

</script>

<template>
  <Header />
  <!-- <RegisterForm /> -->
  <CarLoan />
   <!-- <Pokedex /> -->
  
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

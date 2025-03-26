<script setup>
import { ref } from 'vue';

// import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
    <header>
        <a href="#">Home</a>
        <a href="#">About</a>
        <a href="#">Contact</a>
    </header>
</template>

<style scoped>
  header{
    display: flex;
    justify-content: center;
    align-items: center;
  }
  a{
    font-size: 1.1rem;
    padding: 10px 20px;
    display: block;
  }
</style>

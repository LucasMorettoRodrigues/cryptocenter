<script setup>
import { RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue'
import FooterComponent from './components/FooterComponent.vue'
</script>

<template>
  <Navbar />
  <RouterView />
  <FooterComponent />
</template>

<style>
@import '@/assets/base.css';

.main-container {
  max-width: 1200px;
  margin: 0 auto;
}

.flex {
  display: flex;
}

.column {
  flex-direction: column;
}

.align-center {
  align-items: center;
}

.text-align-center {
  text-align: center;
}

.justify-center {
  justify-content: center;
}

.space-between {
  justify-content: space-between;
}

li {
  list-style: none;
}

a {
  text-decoration: none;
}

.input input {
  padding: 10px;
  border: 1px solid rgb(216, 216, 216);
  outline-color: lightblue;
  margin-bottom: 10px;
}
</style>

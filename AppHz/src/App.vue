<script setup>
import { RouterLink, RouterView } from 'vue-router'

</script>

<template>
  <div id="main" class="container-fluid">
    <div class="p-5 bg-info text-white text-center">
      <h1>My shingles risk</h1>
    </div>

    <nav class="navbar navbar-expand-sm bg-dark navbar-dark">
      <div class="container-fluid">
        <ul class="navbar-nav">
          <li class="nav-item">
            <RouterLink to="/" class="nav-link active">Home</RouterLink>
          </li>
          <li class="nav-item">
            <RouterLink to="/about" class="nav-link active">About</RouterLink>
          </li>
        </ul>
      </div>
    </nav>

    <div class="container">
      <router-view/>
    </div>

    <div class="mt-5 p-4 bg-dark text-white text-center">
      <p>Footer</p>
    </div>
  </div>
</template>

<style scoped>
#main {
  float: none;
}
</style>

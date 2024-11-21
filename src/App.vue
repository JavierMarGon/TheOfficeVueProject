<script setup>
import { RouterLink, RouterView } from 'vue-router'
import "./../src/assets/mainStyles.scss";
</script>

<template>
  <header>
    
    <RouterLink to="/"><img alt="Vue logo" class="logo" src="@/assets/logo.png"  height="100vh" /></RouterLink>
    <div class="wrapper">
      <nav>
        <RouterLink to="/seasons">Seasons</RouterLink>
        <RouterLink to="/chapters">Chapters</RouterLink>
        <RouterLink to="/characters">Characters</RouterLink>
      </nav>
    </div>
  </header>
  <div class="page-container">
    <RouterView/>
  </div>
</template>

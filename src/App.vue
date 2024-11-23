<script setup>
import { RouterLink, RouterView } from 'vue-router'
import "./../src/assets/mainStyles.scss";
</script>

<template>
  <header class="navbar navbar-expand-lg  mb-3" style="background-color: #27140e">
  <div class="container-fluid">
    <RouterLink to="/" class="navbar-brand">
      <img alt="Logo" src="@/assets/logo.png" width="50%" >
    </RouterLink>

    <button class="navbar-toggler  bg-light" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item">
          <RouterLink to="/seasons" class="nav-link" style="color: #aaaaaa;">Seasons</RouterLink>
        </li>
        <li class="nav-item">
          <RouterLink to="/chapters" class="nav-link" style="color: #aaaaaa;">Chapters</RouterLink>
        </li>
        <li class="nav-item">
          <RouterLink to="/characters" class="nav-link" style="color: #aaaaaa;">Characters</RouterLink>
        </li>
      </ul>
    </div>
  </div>
</header>
  <div class="page-container">
    <RouterView />
  </div>
</template>

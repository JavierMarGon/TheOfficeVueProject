<script setup>
import { RouterLink, RouterView } from 'vue-router'
import "./../src/assets/mainStyles.scss";
</script>

<template>
  <div class="d-flex flex-column min-vh-100">
    <!-- Header -->
    <header class="navbar navbar-expand-lg mb-3" style="background-color: #27140e;">
      <div class="container-fluid">
        <RouterLink to="/" class="navbar-brand">
          <img alt="Logo" src="@/assets/logo.png" width="50%">
        </RouterLink>

        <button class="navbar-toggler bg-light" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
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

    <!-- Contenido Principal -->
    <div class="flex-grow-1">
      <RouterView />
    </div>

    <!-- Footer -->
    <footer class="navbar navbar-expand-lg mt-auto" style="background-color: #27140e; color: #aaaaaa;">
      <div class="container-fluid">
        <div class="d-flex flex-column flex-lg-row w-100 justify-content-between align-items-center">
          <!-- Logo o Texto -->
          <RouterLink to="/" class="navbar-brand">
            <img alt="Logo" src="@/assets/logo.png" width="40%">
          </RouterLink>

          <!-- Enlaces del Footer -->
          <ul class="navbar-nav d-flex flex-row justify-content-center mt-3 mt-lg-0">
            <li class="nav-item">
              <p class="nav-link px-3" style="color: #aaaaaa;">Políticas de privacidad</p>
            </li>
            <li class="nav-item">
              <p class="nav-link px-3" style="color: #aaaaaa;">Servicio</p>
            </li>
            <li class="nav-item">
              <p class="nav-link px-3" style="color: #aaaaaa;">Contacto</p>
            </li>
          </ul>

          <!-- Derechos Reservados -->
          <div class="text-center mt-3 mt-lg-0">
            <p class="mb-0" style="font-size: 0.9rem;">© 2024 TheOfficeVueProject. All Rights Reserved.</p>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>


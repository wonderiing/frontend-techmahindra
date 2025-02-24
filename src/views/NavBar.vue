<script setup>
import { ref } from 'vue';

const isMenuOpen = ref(false);

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};
</script>

<template>
  <nav class="navbar">
    <div class="navbar-content">
      <div class="navbar-logo">
        <span class="material-icons">work</span>
        <span class="logo-text">Filter CVs</span>
      </div>
      <div class="navbar-links" :class="{ 'active': isMenuOpen }">
        <a href="#" class="nav-link">Inicio</a>
      </div>
      <button class="menu-toggle" @click="toggleMenu">
        <span class="material-icons">{{ isMenuOpen ? 'close' : 'menu' }}</span>
      </button>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  background-color: #1976d2;
  color: white;
  padding: 0.5rem 1rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.navbar-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
}

.navbar-logo {
  display: flex;
  align-items: center;
  font-size: 1.5rem;
  font-weight: 500;
}

.logo-text {
  margin-left: 0.5rem;
}

.navbar-links {
  display: flex;
  gap: 1rem;
}

.nav-link {
  color: white;
  text-decoration: none;
  font-weight: 500;
  transition: opacity 0.3s;
}

.nav-link:hover {
  opacity: 0.8;
}

.menu-toggle {
  display: none;
  background: none;
  border: none;
  color: white;
  cursor: pointer;
}

@media (max-width: 768px) {
  .navbar-links {
    display: none;
    flex-direction: column;
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background-color: #1976d2;
    padding: 1rem;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .navbar-links.active {
    display: flex;
  }

  .menu-toggle {
    display: block;
  }
}
</style>
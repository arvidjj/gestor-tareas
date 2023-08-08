<template>
  <div id="app">

    <header>
      <div class="header-container">
        <h1>Gestor de Tareas</h1>
        <nav>
          <router-link to="/">Inicio</router-link>
          <router-link to="/about">About</router-link>
        </nav>
      </div>
    </header>

    <main>
      <router-view></router-view>
    </main>
    
  </div>
</template>

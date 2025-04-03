<script setup lang="ts">
// This is where you can import components and setup Vue 3 logic
</script>

<template>
  <div class="app">
    <header>
      <div class="container">
        <h1 class="logo">Vue Interview App</h1>
        <nav>
          <router-link to="/" class="nav-link">Home</router-link>
          <router-link to="/about" class="nav-link">About</router-link>
        </nav>
      </div>
    </header>
    <main class="container">
      <router-view />
    </main>
    <footer>
      <div class="container">
        <p>Vue 3 + TypeScript Frontend Developer Interview Test</p>
      </div>
    </footer>
  </div>
</template>

<style lang="scss">
body {
  margin: 0;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #f5f5f5;
  color: #2c3e50;
}

.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

header {
  background-color: #2c3e50;
  color: white;
  padding: 20px 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  
  .container {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .logo {
    margin: 0;
    font-size: 1.6rem;
    font-weight: bold;
  }
  
  nav {
    display: flex;
    gap: 20px;
    
    .nav-link {
      color: white;
      text-decoration: none;
      font-weight: 500;
      padding: 5px 10px;
      border-radius: 4px;
      transition: background-color 0.2s;
      
      &:hover {
        background-color: rgba(255, 255, 255, 0.1);
      }
      
      &.router-link-active {
        color: #42b983;
        font-weight: 600;
      }
    }
  }
}

main {
  flex: 1;
  padding: 30px 0;
}

footer {
  background-color: #2c3e50;
  color: white;
  padding: 20px 0;
  text-align: center;
  font-size: 0.9rem;
  margin-top: auto;
  
  p {
    margin: 0;
    opacity: 0.8;
  }
}
</style> 
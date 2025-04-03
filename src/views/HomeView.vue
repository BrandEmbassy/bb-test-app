<script setup lang="ts">
import MessyUserList from '@/components/MessyUserList.vue'
</script>

<template>
  <div class="home">
    <h1>Frontend Developer Interview Task</h1>
    <p class="description">
      This component has several issues that need to be addressed to improve code quality.
      Review the code and make the necessary improvements to follow Vue.js best practices.
    </p>
    <MessyUserList />
  </div>
</template>

<style lang="scss" scoped>
.home {
  text-align: center;
  padding: 20px;
  
  h1 {
    color: #42b983;
  }

  .description {
    max-width: 800px;
    margin: 0 auto 30px;
    color: #666;
    line-height: 1.5;
    font-style: italic;
    padding: 15px;
    background-color: #f9f9f9;
    border-radius: 8px;
    border-left: 4px solid #42b983;
  }
}
</style> 
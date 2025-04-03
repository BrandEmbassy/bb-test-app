<script setup lang="ts">
// Component logic using script setup
</script>

<template>
  <div class="about">
    <h1>About This Interview Task</h1>
    <div class="content">
      <section>
        <h2>Code Review Exercise</h2>
        <p>
          The home page includes a component with code that needs improvement.
          This represents common scenarios you might encounter when working with Vue applications.
        </p>
      </section>
      
      <section>
        <h2>Assessment Criteria</h2>
        <p>
          During the interview, we're looking for your ability to:
        </p>
        <ul>
          <li>Identify code quality issues</li>
          <li>Apply Vue.js best practices</li>
          <li>Refactor code for better maintainability</li>
          <li>Structure components appropriately</li>
          <li>Understand reactivity in Vue</li>
        </ul>
      </section>
      
      <section>
        <h2>Instructions</h2>
        <p>
          Review the component in the home page. Identify the issues and explain how you would 
          refactor the code to follow best practices. Feel free to sketch out your solutions.
        </p>
      </section>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.about {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  
  h1 {
    color: #2c3e50;
    text-align: center;
    margin-bottom: 40px;
  }
  
  .content {
    background-color: white;
    border-radius: 8px;
    padding: 30px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    
    section {
      margin-bottom: 25px;
      
      &:last-child {
        margin-bottom: 0;
      }
      
      h2 {
        color: #42b983;
        margin-bottom: 15px;
        font-size: 1.5rem;
        border-bottom: 1px solid #eee;
        padding-bottom: 8px;
      }
      
      p {
        line-height: 1.6;
        color: #444;
      }
      
      ul {
        padding-left: 20px;
        
        li {
          margin-bottom: 8px;
          color: #444;
          line-height: 1.5;
        }
      }
    }
  }
}
</style> 
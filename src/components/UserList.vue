<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'

interface User {
  id: number
  name: string
  email: string
}

const users = ref<User[]>([])
const loading = ref(true)
const error = ref<string | null>(null)

const fetchUsers = async () => {
  try {
    loading.value = true
    const response = await axios.get('https://jsonplaceholder.typicode.com/users')
    users.value = response.data
  } catch (err) {
    error.value = 'Failed to fetch users'
    console.error(err)
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchUsers()
})
</script>

<template>
  <div class="user-list">
    <h2>User List</h2>
    <div v-if="loading">Loading users...</div>
    <div v-else-if="error">{{ error }}</div>
    <ul v-else class="users">
      <li v-for="user in users" :key="user.id" class="user-item">
        <h3>{{ user.name }}</h3>
        <p>{{ user.email }}</p>
      </li>
    </ul>
  </div>
</template>

<style lang="scss" scoped>
.user-list {
  background-color: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  
  h2 {
    color: #42b983;
    margin-bottom: 20px;
  }
  
  .users {
    list-style: none;
    padding: 0;
    
    .user-item {
      padding: 15px;
      border-bottom: 1px solid #eee;
      
      &:last-child {
        border-bottom: none;
      }
      
      h3 {
        margin: 0 0 5px 0;
      }
      
      p {
        margin: 0;
        color: #666;
      }
    }
  }
}
</style> 
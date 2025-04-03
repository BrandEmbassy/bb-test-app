<script setup lang="ts">
import { ref, onMounted, reactive } from 'vue'
import axios from 'axios'

// Basic user interface
interface User {
  id: number
  name: string
  email: string
  phone: string
  favorite?: boolean
}

const users = ref<User[]>([])
const loading = ref(true)
const error = ref<string | null>(null)
const selectedUser = ref<User | null>(null)
const favoriteCount = ref(0)

// Callback hell example for refactoring with async/await
const fetchUsers = () => {
  loading.value = true
  axios.get('https://jsonplaceholder.typicode.com/users')
    .then(response => {
      users.value = response.data
      return axios.get('https://jsonplaceholder.typicode.com/todos?userId=1')
    })
    .then(todosResponse => {
      console.log(`User 1 has ${todosResponse.data.length} todos`)
      return axios.get('https://jsonplaceholder.typicode.com/posts?userId=1')
    })
    .then(postsResponse => {
      console.log(`User 1 has ${postsResponse.data.length} posts`)
      loading.value = false
    })
    .catch(err => {
      error.value = 'Failed to fetch data'
      console.error(err)
      loading.value = false
    })
}

const selectUser = (user: User) => {
  selectedUser.value = user
}

// Subtle reactivity problem: Direct mutation of array item property
// without triggering proper reactivity updates
const toggleFavorite = (user: User) => {
  // The reactivity issue: directly modifying a nested property without proper Vue reactivity
  user.favorite = !user.favorite
  
  // Count favorites (this won't reliably update the UI because of the reactivity issue)
  favoriteCount.value = users.value.filter(u => u.favorite).length
}

onMounted(() => {
  fetchUsers()
})
</script>

<template>
  <div class="user-list-container">
    <h2>User Directory</h2>
    
    <!-- Loading and error states -->
    <div v-if="loading" class="loading-spinner">
      <div class="spinner"></div>
      <p>Loading users...</p>
    </div>
    
    <div v-else-if="error" class="error-message">
      {{ error }}
      <button @click="fetchUsers">Try Again</button>
    </div>
    
    <div v-else>
      <div class="favorite-count">
        Favorite users: {{ favoriteCount }}
      </div>
      
      <!-- All user list rendering in one place - candidate should extract to component -->
      <div class="users-container">
        <p class="results-count">
          Found {{ users.length }} users
        </p>
        
        <!-- User list that should be a separate component -->
        <div class="user-cards">
          <!-- User card that should be a separate component -->
          <div 
            v-for="user in users" 
            :key="user.id" 
            class="user-card"
            :class="{ selected: selectedUser && selectedUser.id === user.id, favorite: user.favorite }"
          >
            <div class="user-header">
              <h3>{{ user.name }}</h3>
              <span class="user-id">#{{ user.id }}</span>
            </div>
            
            <div class="user-contact">
              <p><strong>Email:</strong> {{ user.email }}</p>
              <p><strong>Phone:</strong> {{ user.phone }}</p>
            </div>
            
            <div class="card-actions">
              <button class="view-details-btn" @click="selectUser(user)">
                View Details
              </button>
              <button 
                class="favorite-btn"
                @click.stop="toggleFavorite(user)"
                :class="{ active: user.favorite }"
              >
                {{ user.favorite ? '★' : '☆' }}
              </button>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Selected user details -->
      <div v-if="selectedUser" class="user-details">
        <h3>Selected User: {{ selectedUser.name }}</h3>
        <p><strong>Email:</strong> {{ selectedUser.email }}</p>
        <p><strong>Phone:</strong> {{ selectedUser.phone }}</p>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
// Overly nested and duplicated styling
.user-list-container {
  background-color: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  margin-bottom: 30px;
  
  h2 {
    color: #42b983;
    margin-bottom: 20px;
    text-align: center;
  }
  
  .favorite-count {
    background-color: #f8f9fa;
    padding: 10px;
    border-radius: 4px;
    margin-bottom: 20px;
    text-align: center;
    font-weight: bold;
    color: #42b983;
  }
  
  .loading-spinner {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 40px 0;
    
    .spinner {
      border: 4px solid rgba(0, 0, 0, 0.1);
      border-radius: 50%;
      border-top: 4px solid #42b983;
      width: 40px;
      height: 40px;
      animation: spin 1s linear infinite;
    }
    
    p {
      margin-top: 15px;
      color: #666;
    }
  }
  
  .error-message {
    background-color: #ffebee;
    color: #c62828;
    padding: 15px;
    border-radius: 4px;
    text-align: center;
    margin: 20px 0;
    
    button {
      background-color: #c62828;
      color: white;
      border: none;
      padding: 8px 15px;
      border-radius: 4px;
      margin-top: 10px;
      cursor: pointer;
      
      &:hover {
        background-color: #b71c1c;
      }
    }
  }
  
  .users-container {
    .results-count {
      color: #666;
      margin-bottom: 15px;
      font-size: 14px;
    }
    
    .user-cards {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 20px;
      
      .user-card {
        background-color: #f9f9f9;
        border-radius: 8px;
        padding: 15px;
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
        cursor: pointer;
        transition: transform 0.2s, box-shadow 0.2s;
        
        &:hover {
          transform: translateY(-5px);
          box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        &.selected {
          border: 2px solid #42b983;
        }
        
        &.favorite {
          background-color: #fff8e1;
        }
        
        .user-header {
          display: flex;
          justify-content: space-between;
          align-items: baseline;
          margin-bottom: 10px;
          border-bottom: 1px solid #eee;
          padding-bottom: 10px;
          
          h3 {
            margin: 0;
            color: #2c3e50;
          }
          
          .user-id {
            color: #999;
            font-size: 14px;
          }
        }
        
        .user-contact {
          margin-bottom: 10px;
          
          p {
            margin: 5px 0;
            font-size: 14px;
            word-break: break-word;
          }
        }
        
        .card-actions {
          display: flex;
          gap: 10px;
          
          .view-details-btn {
            flex: 1;
            background-color: #42b983;
            border: none;
            color: white;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            
            &:hover {
              background-color: #3aa876;
            }
          }
          
          .favorite-btn {
            width: 40px;
            background-color: #f5f5f5;
            border: 1px solid #ddd;
            border-radius: 4px;
            cursor: pointer;
            font-size: 18px;
            
            &:hover {
              background-color: #eeeeee;
            }
            
            &.active {
              background-color: #ffd700;
              color: #b8860b;
              border-color: #daa520;
            }
          }
        }
      }
    }
  }
  
  .user-details {
    background-color: #f5f5f5;
    border-radius: 8px;
    margin-top: 30px;
    padding: 20px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
    
    h3 {
      color: #42b983;
      margin-top: 0;
      margin-bottom: 15px;
      border-bottom: 1px solid #ddd;
      padding-bottom: 10px;
    }
    
    p {
      margin: 8px 0;
      font-size: 14px;
    }
  }
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style> 
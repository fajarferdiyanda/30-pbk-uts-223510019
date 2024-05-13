<template>
  <div class="container">
    <header>
      <nav>
        <ul>
          <li @click="selectedMenu = 'Post'" :class="{ active: selectedMenu === 'Post' }">Post</li>
          <li @click="selectedMenu = 'Todos'" :class="{ active: selectedMenu === 'Todos' }">Todos</li>
        </ul>
      </nav>
    </header>
    <div class="content">
      <div v-if="selectedMenu === 'Post'" class="post-section">
        <select v-model="selectedUser" @change="fetchPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
        <div v-if="loadingPosts">
          <p>Loading...</p>
        </div>
        <div v-else-if="posts.length > 0">
          <h2>Postingan User: {{ selectedUserName }}</h2>
          <ul>
            <li v-for="post in posts" :key="post.id">
              <h3>{{ post.title }}</h3>
              <p>{{ post.body }}</p>
            </li>
          </ul>
        </div>
        <div v-else>
          <p>Tidak ada postingan untuk user ini.</p>
        </div>
      </div>
      <div v-else-if="selectedMenu === 'Todos'" class="todos-section">
        <Todos :todos="todos" />
      </div>
    </div>
  </div>
</template>

<script>
import Todos from './Todos.vue'; // Sesuaikan dengan path file komponen Todos Anda

export default {
  components: {
    Todos
  },
  data() {
    return {
      selectedMenu: 'Post', // Default menu selection
      users: [],
      selectedUser: null,
      selectedUserName: '',
      posts: [],
      todos: [], // Isi dengan data Todos yang telah Anda dapatkan
      loadingPosts: false,
      errorPosts: null
    };
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        this.users = await response.json();
      } catch (error) {
        console.error('Error fetching users:', error);
      }
    },
    async fetchPosts() {
      if (!this.selectedUser) return;
      this.loadingPosts = true;
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
        this.posts = await response.json();
        const selectedUser = this.users.find(user => user.id === parseInt(this.selectedUser));
        if (selectedUser) {
          this.selectedUserName = selectedUser.name;
        }
        this.loadingPosts = false;
      } catch (error) {
        console.error('Error fetching posts:', error);
        this.errorPosts = 'Terjadi kesalahan saat mengambil data postingan.';
        this.loadingPosts = false;
      }
    }
  },
  watch: {
    selectedUser() {
      this.fetchPosts();
    }
  },
  created() {
    this.fetchUsers();
  }
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.content {
  max-width: 800px;
}

header {
  background-color: #333;
  padding: 10px 0;
}

nav ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

nav ul li {
  display: inline-block;
  margin-right: 20px;
  color: #fff;
  cursor: pointer;
}

nav ul li.active {
  font-weight: bold;
}

.post-section,
.todos-section {
  margin-top: 20px;
}
</style>

<template>
  <div class="container">
    <header>
      <nav>
        <ul>
          <li>
            <router-link to="/posts">Posts</router-link>
          </li>
          <li>
            <router-link to="/todos">Todos</router-link>
          </li>
          <li>
            <router-link to="/albums">Albums</router-link>
          </li>
        </ul>
      </nav>
    </header>
    <div class="content">
      <router-view />
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
  min-height: 100vh;
}

header {
  background-color: #000101;
  padding: 10px;
  border-radius: 5px;
  width: 100%;
  max-width: 800px;
}

nav ul {
  list-style: none;
  display: flex;
  justify-content: space-around;
  padding: 0;
  margin: 0;
}

nav ul li {
  margin: 0 5px;
}

router-link {
  color: #ece0e0;
  text-decoration: none;
  padding: 5px 10px;
  border-radius: 3px;
  transition: background-color 0.3s;
}

router-link:hover {
  background-color: #efe5ef;
}

.content {
  width: 100%;
  max-width: 800px;
  margin-top: 20px;
}


</style>

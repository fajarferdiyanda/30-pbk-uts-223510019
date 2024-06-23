<template>
    <div>
      <h1>Albums</h1>
      <ul class="album-list">
        <li v-for="album in albums" :key="album.id">
          <router-link :to="`/albums/${album.id}`">{{ album.title }}</router-link>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup>
  import { onMounted, computed } from 'vue';
  import { useAlbumStore } from '../stores/albumStore';
  
  const albumStore = useAlbumStore();
  
  onMounted(() => {
    albumStore.fetchAlbums();
  });
  
  const albums = computed(() => albumStore.albums);
  </script>
  
  <style>
  .album-list {
    list-style-type: none;
    padding: 0;
  }
  
  .album-list li {
    margin: 5px 0;
  }
  
  .album-list li a {
    text-decoration: none;
    color: #3A82EE;
    padding: 5px;
    border-radius: 3px;
    transition: background-color 0.3s;
  }
  
  .album-list li a:hover {
    background-color: #f0f0f0;
  }
  </style>
  
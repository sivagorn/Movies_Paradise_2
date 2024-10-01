<template>
    <AppLayout>
      <div class="container">
        <div v-if="loading" class="loader">กำลังโหลดโปรดรอสักครู่...</div>
        <div v-else class="movie-grid">
          <div 
            v-for="popular in state.populars" 
            :key="popular.index" 
            class="card" 
            @click="goToDetail(popular.index)"
          >
            <img 
              class="poster" 
              :src="popular.poster" 
              alt="Poster"
            />
            <div class="card-content">
              <p class="movie-name">{{ popular.title }}</p>
              <p class="movie-count">เข้าชมแล้ว {{ popular.count }} ครั้ง</p>
            </div>
          </div>
        </div>
      </div>
    </AppLayout>
  </template>
  
  <script setup>
  import { onMounted, reactive, ref } from 'vue';
  import { useRouter } from 'vue-router';
  import axios from 'axios';
  
  const router = useRouter();
  
  const state = reactive({
    populars: []
  });
  
  const loading = ref(true);
  
  const goToDetail = (index) => {
    router.push({ name: 'PopularDetail', params: { index } });
  };
  
  onMounted(() => {
    const url = 'https://script.google.com/macros/s/AKfycbxOTQ8A8b9yGrHCs-QCzKuLaPCktM6-mwXMpjYW9z8uhuPV8RFRVdJ5nlPQY84j-xzGPg/exec';
    
    axios.get(url)
      .then((response) => {
        state.populars = response.data; 
        localStorage.setItem('populars', JSON.stringify(state.populars)); 
      })
      .finally(() => {
        loading.value = false; 
      });
  });
  </script>
  
  <style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Itim&display=swap'); 
  
  .container {
    margin-top: 50px;
    margin-bottom: 70px;
  }
  
  .movie-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr); 
    gap: 2rem; 
  }
  
  .card {
    display: flex;
    flex-direction: column;
    border: 5px solid #000000;
    box-sizing: border-box;
    background-color: #ffffff;
    transition: transform 0.3s ease, box-shadow 0.3s ease; 
  }
  
  .card:hover {
    transform: translateY(-10px); 
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2); 
  }
  
  .poster {
    width: 100%;
    height: auto; 
    object-fit: cover;
  }
  
  .card-content {
    padding: 0.5rem; 
    text-align: center;
  }
  
  .movie-name {
    padding-top: 15px;
    padding-bottom: 5px;
    font-family: "Itim", sans-serif; 
    font-weight: normal; 
    font-size: 22px; 
    margin: 0;
    color: rgb(0, 0, 0);
    text-align: center; 
  }
  
  .movie-count {
    font-family: "Itim", sans-serif; 
    font-size: 20px; 
    color: rgb(0, 0, 0);
  }
  
  .loader {
    text-align: center;
    font-size: 24px;
    color: #000;
  }
  
  @media (max-width: 1024px) {
    .movie-grid {
      grid-template-columns: repeat(4, 1fr); 
    }
  }
  
  @media (max-width: 768px) {
    .movie-grid {
      grid-template-columns: repeat(4, 1fr); 
    }
  }
  
  @media (max-width: 480px) {
    .movie-grid {
      grid-template-columns: 1fr; 
    }
  
    .card {
      max-width: 300px; 
      margin: 0 auto; 
    }
  
    .movie-name {
      font-size: 20px; 
    }
  
    .movie-count {
      font-size: 18px; 
    }
  
    .card-content {
      padding: 0.5rem; 
      text-align: center;
    }
  }
  </style>
  
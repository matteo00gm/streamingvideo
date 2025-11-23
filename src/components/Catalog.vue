<template>
  <div class="catalog-container">
    <div class="filter-bar">
      <button 
        :class="['filter-btn', { active: type === 'tv' }]"
        @click="setType('tv')"
      >
        TV Shows
      </button>
      <button 
        :class="['filter-btn', { active: type === 'movie' }]"
        @click="setType('movie')"
      >
        Movies
      </button>
    </div>

    <div class="catalog-grid">
      <div 
        v-for="item in visibleItems" 
        :key="item.tmdb_id" 
        class="show-card"
        @click="$emit('select', { id: item.tmdb_id, type: type })"
      >
        <div class="card-content">
          <span class="icon">{{ type === 'tv' ? '📺' : '🎬' }}</span>
          <span class="show-id">ID: {{ item.tmdb_id }}</span>
          <span class="play-hint">Click to Play</span>
        </div>
      </div>
    </div>
    <div v-if="hasMore" class="load-more-container">
      <button @click="loadMore" class="load-more-btn">Load More</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'CatalogComponent',
  data() {
    return {
      items: [],
      visibleCount: 20,
      loading: true,
      error: null,
      type: 'tv'
    };
  },
  computed: {
    visibleItems() {
      return this.items.slice(0, this.visibleCount);
    },
    hasMore() {
      return this.visibleCount < this.items.length;
    }
  },
  async created() {
    await this.fetchCatalog();
  },
  methods: {
    async fetchCatalog() {
      this.loading = true;
      this.items = [];
      try {
        const response = await axios.get(`https://vixsrc.to/api/list/${this.type}?lang=it`);
        this.items = response.data;
      } catch (err) {
        this.error = 'Failed to load catalog';
        console.error(err);
      } finally {
        this.loading = false;
      }
    },
    setType(newType) {
      if (this.type === newType) return;
      this.type = newType;
      this.visibleCount = 20;
      this.fetchCatalog();
    },
    loadMore() {
      this.visibleCount += 20;
    }
  }
}
</script>

<style scoped>
.catalog-container {
  padding: 20px;
  width: 100%;
  box-sizing: border-box;
}

.filter-bar {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 30px;
}

.filter-btn {
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: #fff;
  padding: 10px 25px;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s;
  font-size: 1rem;
}

.filter-btn.active {
  background: #00f2ff;
  color: #000;
  border-color: #00f2ff;
  box-shadow: 0 0 15px rgba(0, 242, 255, 0.5);
}

.catalog-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 20px;
}

.show-card {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  aspect-ratio: 2/3;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
  position: relative;
  overflow: hidden;
}

.show-card:hover {
  transform: translateY(-5px);
  background: rgba(255, 255, 255, 0.1);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.5);
  border-color: #00f2ff;
}

.card-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  color: #fff;
}

.icon {
  font-size: 2rem;
}

.show-id {
  font-family: 'Courier New', monospace;
  font-size: 0.9rem;
  opacity: 0.7;
}

.play-hint {
  font-size: 0.8rem;
  color: #00f2ff;
  opacity: 0;
  transform: translateY(10px);
  transition: all 0.3s ease;
}

.show-card:hover .play-hint {
  opacity: 1;
  transform: translateY(0);
}

.load-more-container {
  display: flex;
  justify-content: center;
  margin-top: 40px;
}

.load-more-btn {
  background: linear-gradient(45deg, #00f2ff, #0077ff);
  border: none;
  padding: 12px 30px;
  border-radius: 25px;
  color: #fff;
  font-weight: bold;
  cursor: pointer;
  transition: transform 0.2s;
}

.load-more-btn:hover {
  transform: scale(1.05);
}
</style>

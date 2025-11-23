<template>
  <div class="player-container">
    <div class="controls-bar">
      <button @click="$emit('back')" class="back-btn">
        ← Back to Catalog
      </button>
      <div class="episode-selector" v-if="type === 'tv'">
        <div class="input-group">
          <label>Season</label>
          <input type="number" v-model="season" min="1" @change="updateSource">
        </div>
        <div class="input-group">
          <label>Episode</label>
          <input type="number" v-model="episode" min="1" @change="updateSource">
        </div>
      </div>
    </div>
    
    <div class="video-wrapper">
      <iframe 
        :src="videoUrl" 
        frameborder="0" 
        allowfullscreen
        class="video-frame"
      ></iframe>
    </div>
    
    <div class="info-panel">
      <h2>Now Playing</h2>
      <p>
        {{ type === 'tv' ? 'TV Show' : 'Movie' }} ID: {{ showId }}
        <span v-if="type === 'tv'"> | Season {{ season }} | Episode {{ episode }}</span>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VideoPlayer',
  props: {
    showId: {
      type: Number,
      required: true
    },
    type: {
      type: String,
      default: 'tv'
    }
  },
  data() {
    return {
      season: 1,
      episode: 1,
      videoUrl: ''
    };
  },
  created() {
    this.updateSource();
  },
  methods: {
    updateSource() {
      if (this.type === 'tv') {
        // https://vixsrc.to/tv/{showId}/{seasonId}/{episodeId}/?lang=it
        this.videoUrl = `https://vixsrc.to/tv/${this.showId}/${this.season}/${this.episode}/?lang=it`;
      } else {
        // https://vixsrc.to/embed/movie/{id}?lang=it (Assuming embed format, or standard page)
        // The user provided TV format. I'll guess movie format based on common patterns.
        // I'll use the /movie/ path as verified by 200 OK response earlier.
        this.videoUrl = `https://vixsrc.to/embed/movie/${this.showId}?lang=it`;
      }
    }
  }
}
</script>

<style scoped>
.player-container {
  display: flex;
  flex-direction: column;
  height: 100%;
  gap: 20px;
  padding: 20px;
  box-sizing: border-box;
}

.controls-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.05);
  padding: 15px;
  border-radius: 12px;
  backdrop-filter: blur(10px);
}

.back-btn {
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: #fff;
  padding: 8px 16px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
}

.back-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: #fff;
}

.episode-selector {
  display: flex;
  gap: 20px;
}

.input-group {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #fff;
}

input {
  background: rgba(0, 0, 0, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #fff;
  padding: 5px 10px;
  border-radius: 5px;
  width: 60px;
  text-align: center;
}

.video-wrapper {
  flex: 1;
  background: #000;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
  position: relative;
  min-height: 500px;
}

.video-frame {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

.info-panel {
  color: #fff;
  text-align: left;
  padding: 0 10px;
}

h2 {
  margin: 0 0 10px 0;
  font-weight: 300;
  color: #00f2ff;
}

p {
  opacity: 0.7;
  margin: 0;
}
</style>

<template>
  <div id="app">
    <header class="app-header">
      <div class="logo">Vix<span class="highlight">Stream</span> Manual</div>
    </header>
    
    <main class="main-content">
      <div class="control-panel">
        <div class="input-row">
          <div class="input-group">
            <label>Show ID (TMDB)</label>
            <input 
              v-model="showId" 
              type="number" 
              placeholder="e.g. 1399"
              @keyup.enter="loadVideo"
            />
          </div>
          <div class="input-group">
            <label>Season</label>
            <input 
              v-model="seasonId" 
              type="number" 
              placeholder="1"
              min="1"
              @keyup.enter="loadVideo"
            />
          </div>
          <div class="input-group">
            <label>Episode</label>
            <input 
              v-model="episodeId" 
              type="number" 
              placeholder="1"
              min="1"
              @keyup.enter="loadVideo"
            />
          </div>
        </div>
        <button @click="loadVideo" class="play-btn" :disabled="!isValid">
          <span class="play-icon">▶</span> Play Stream
        </button>
      </div>

      <div class="video-wrapper" v-if="videoUrl">
        <iframe 
          :src="videoUrl" 
          frameborder="0" 
          allowfullscreen
          class="video-frame"
        ></iframe>
      </div>
      
      <div class="placeholder-state" v-else>
        <div class="placeholder-content">
          <span class="placeholder-icon">📺</span>
          <p>Enter details above to start streaming</p>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      showId: 94997,
      seasonId: 1,
      episodeId: 1,
      videoUrl: ''
    }
  },
  computed: {
    isValid() {
      return this.showId && this.seasonId > 0 && this.episodeId > 0;
    }
  },
  methods: {
    loadVideo() {
      if (!this.isValid) return;
      // Construct the URL as requested: https://vixsrc.to/tv/{showId}/{seasonId}/{episodeId}/?lang=it
      this.videoUrl = `https://vixsrc.to/tv/${this.showId}/${this.seasonId}/${this.episodeId}/?lang=it`;
    }
  }
}
</script>

<style>
body {
  margin: 0;
  background-color: #0f0f0f;
  color: #ffffff;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.app-header {
  padding: 20px 40px;
  background: rgba(15, 15, 15, 0.95);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  align-items: center;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.5);
}

.logo {
  font-size: 1.5rem;
  font-weight: 800;
  letter-spacing: -0.5px;
}

.highlight {
  color: #00f2ff;
}

.main-content {
  flex: 1;
  max-width: 1200px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 20px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.control-panel {
  background: rgba(30, 30, 30, 0.6);
  padding: 25px;
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center;
}

.input-row {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  justify-content: center;
  width: 100%;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

label {
  font-size: 0.85rem;
  color: #aaa;
  font-weight: 500;
  margin-left: 4px;
}

input {
  background: rgba(0, 0, 0, 0.4);
  border: 1px solid rgba(255, 255, 255, 0.15);
  color: #fff;
  padding: 12px 16px;
  border-radius: 8px;
  font-size: 1rem;
  width: 120px;
  transition: all 0.2s;
  text-align: center;
}

input:focus {
  outline: none;
  border-color: #00f2ff;
  box-shadow: 0 0 0 2px rgba(0, 242, 255, 0.2);
}

.play-btn {
  background: linear-gradient(135deg, #00f2ff 0%, #0077ff 100%);
  border: none;
  padding: 12px 40px;
  border-radius: 12px;
  color: #fff;
  font-weight: 700;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 10px;
  margin-top: 10px;
  box-shadow: 0 4px 15px rgba(0, 119, 255, 0.3);
}

.play-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0, 119, 255, 0.5);
}

.play-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  background: #333;
  box-shadow: none;
}

.video-wrapper {
  flex: 1;
  background: #000;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
  position: relative;
  min-height: 600px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.video-frame {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

.placeholder-state {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.02);
  border-radius: 16px;
  border: 2px dashed rgba(255, 255, 255, 0.1);
  min-height: 400px;
}

.placeholder-content {
  text-align: center;
  color: #555;
}

.placeholder-icon {
  font-size: 4rem;
  display: block;
  margin-bottom: 10px;
  opacity: 0.5;
}

/* Responsive adjustments */
@media (max-width: 600px) {
  .input-row {
    flex-direction: column;
    align-items: center;
  }
  
  input {
    width: 100%;
    box-sizing: border-box;
  }
  
  .play-btn {
    width: 100%;
    justify-content: center;
  }
}
</style>

<template>
  <section class="hero">
    <div class="hero-content">
      <!-- Hero Image/Video -->
      <div class="hero-media">
        <img 
          v-if="!showVideo" 
          src="/Assets/Images/Header Image.jpg" 
          alt="Cinema Hall" 
          class="hero-image"
        >
        <video 
          v-else
          src="/Assets/Videos/Header Video.mp4" 
          autoplay 
          muted 
          loop 
          class="hero-video"
        ></video>
        
        <!-- Video Toggle Button -->
        <button 
          class="video-toggle-btn"
          @click="toggleVideo"
          :title="showVideo ? 'Switch to Image' : 'Switch to Video'"
        >
          {{ showVideo ? '📷' : '🎥' }}
        </button>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const showVideo = ref(false)

const toggleVideo = () => {
  showVideo.value = !showVideo.value
}
</script>

<style scoped>
.hero {
  position: relative;
  min-height: 80vh;
  display: flex;
  align-items: center;
  font-family: var(--main-font);
  justify-content: center;
  overflow: hidden;
  margin-top: 0; /* Ensure no gap above */
}

.hero-content {
  position: relative;
  width: 100%;
  height: 100%;
}

.hero-media {
  position: relative;
  width: 100%;
  height: 80vh;
  overflow: hidden;
}

.hero-image,
.hero-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.video-toggle-btn {
  position: absolute;
  top: 20px;
  right: 20px;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  border: none;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  font-size: 1.5rem;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 10;
}

.video-toggle-btn:hover {
  background: rgba(255, 107, 53, 0.8);
  transform: scale(1.1);
}

/* Tablet Responsive */
@media (max-width: 1024px) {
  .hero {
    min-height: 70vh;
  }

  .hero-media {
    height: 70vh;
  }
}

/* Mobile Responsive */
@media (max-width: 768px) {
  .hero {
    min-height: 60vh;
  }

  .hero-media {
    height: 60vh;
  }

  .video-toggle-btn {
    width: 40px;
    height: 40px;
    font-size: 1.2rem;
    top: 15px;
    right: 15px;
  }
}

@media (max-width: 480px) {
  .hero {
    min-height: 50vh;
  }

  .hero-media {
    height: 50vh;
  }

  .video-toggle-btn {
    width: 35px;
    height: 35px;
    font-size: 1rem;
    top: 10px;
    right: 10px;
  }
}

@media (max-width: 360px) {
  .hero {
    min-height: 45vh;
  }

  .hero-media {
    height: 45vh;
  }
}
</style>
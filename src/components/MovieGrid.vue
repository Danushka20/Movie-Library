<template>
  <section class="movie-grid-section">
    <div class="container">
      <!-- Section Header -->
      <div class="section-header">
        <h2 class="section-title">Collect your favourites</h2>
        <div class="search-container">
          <div class="search-input-wrapper">
            <img src="../assets/Icons/Search White.svg" alt="Search" class="search-icon">
            <input 
              type="text" 
              v-model="searchQuery" 
              @input="handleSearch"
              placeholder="Search title and add to grid"
              class="search-input"
            >
          </div>
        </div>
      </div>

      <!-- Search Results -->
      <div v-if="searchResults.length > 0 && searchQuery" class="search-results">
        <h3>Search Results</h3>
        <div class="search-results-grid">
          <div 
            v-for="movie in searchResults" 
            :key="movie.id" 
            class="search-result-card"
            @click="addToGrid(movie)"
          >
            <img 
              :src="movie.image?.medium || require('../assets/Images/Batman.jpg')" 
              :alt="movie.name"
              class="search-result-image"
              @error="handleImageError"
            >
            <div class="search-result-info">
              <h4>{{ movie.name }}</h4>
              <p>{{ movie.summary?.replace(/<[^>]*>/g, '').substring(0, 100) }}...</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Movie Grid -->
      <div class="movie-grid">
        <div 
          v-for="movie in selectedMovies" 
          :key="movie.id" 
          class="movie-card"
        >
          <div class="movie-image-container">
            <img 
              :src="movie.image?.medium || require('../assets/Images/Batman.jpg')" 
              :alt="movie.name"
              class="movie-image"
              @error="handleImageError"
            >
            <button 
              @click.stop="removeFromGrid(movie.id)"
              class="remove-btn"
              title="Remove from grid"
            >
              <img src="../assets/Icons/Close White.svg" alt="Remove" class="remove-icon">
            </button>
          </div>
          <div class="movie-info">
            <h3 class="movie-title">{{ movie.name }}</h3>
            <p class="movie-description">
              {{ movie.summary?.replace(/<[^>]*>/g, '').substring(0, 150) || 'No description available.' }}
            </p>
          </div>
        </div>
      </div>

      <!-- Empty State -->
      <div v-if="selectedMovies.length === 0" class="empty-state">
        <p>No movies selected yet. Search for movies to add them to your collection!</p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const searchQuery = ref('')
const searchResults = ref([])
const selectedMovies = ref([])
const searchTimeout = ref(null)

// Fetch initial movies from TVMaze API
const fetchInitialMovies = async () => {
  try {
    const response = await fetch('https://api.tvmaze.com/shows')
    const data = await response.json()
    // Take first 3 movies as default
    selectedMovies.value = data.slice(0, 3).map(movie => ({
      id: movie.id,
      name: movie.name,
      image: movie.image,
      summary: movie.summary
    }))
  } catch (error) {
    console.error('Error fetching initial movies:', error)
    // Fallback to hardcoded movies if API fails
    selectedMovies.value = [
      {
        id: 1,
        name: 'Batman Returns',
        image: { medium: require('../assets/Images/Batman.jpg') },
        summary: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.'
      },
      {
        id: 2,
        name: 'Wild Wild West',
        image: { medium: require('../assets/Images/Wild West.jpg') },
        summary: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.'
      },
      {
        id: 3,
        name: 'The Amazing Spiderman',
        image: { medium: require('../assets/Images/Spiderman.jpg') },
        summary: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.'
      }
    ]
  }
}

onMounted(() => {
  fetchInitialMovies()
})

const handleSearch = () => {
  if (searchTimeout.value) {
    clearTimeout(searchTimeout.value)
  }

  if (searchQuery.value.trim().length < 2) {
    searchResults.value = []
    return
  }

  searchTimeout.value = setTimeout(async () => {
    try {
      const response = await fetch(`https://api.tvmaze.com/search/shows?q=${encodeURIComponent(searchQuery.value)}`)
      const data = await response.json()
      searchResults.value = data.slice(0, 6).map(item => item.show) // Limit to 6 results
    } catch (error) {
      console.error('Error fetching movies:', error)
      searchResults.value = []
    }
  }, 300)
}

const addToGrid = (movie) => {
  // Check if movie is already in the grid
  const exists = selectedMovies.value.find(m => m.id === movie.id)
  if (!exists) {
    selectedMovies.value.push({
      id: movie.id,
      name: movie.name,
      image: movie.image,
      summary: movie.summary
    })
  }
  searchQuery.value = ''
  searchResults.value = []
}

const removeFromGrid = (movieId) => {
  selectedMovies.value = selectedMovies.value.filter(movie => movie.id !== movieId)
}

const handleImageError = (event) => {
  event.target.src = require('../assets/Images/Batman.jpg')
}
</script>

<style scoped>
.movie-grid-section {
  padding: 5rem 0;
  background-color: #2d2d2d; /* Changed from #000000 to gray */
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4rem;
  flex-wrap: wrap;
  gap: 2.5rem;
}

.section-title {
  font-size: 3rem;
  font-weight: bold;
  color: #ffffff;
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 2px;
}

.search-container {
  flex: 1;
  max-width: 450px;
}

.search-input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.search-icon {
  position: absolute;
  left: 15px;
  width: 20px;
  height: 20px;
  filter: invert(1);
  z-index: 2;
}

.search-input {
  width: 100%;
  padding: 15px 15px 15px 45px;
  border: 2px solid #333;
  border-radius: 8px;
  background-color: #1a1a1a;
  color: white;
  font-size: 1.1rem;
  transition: border-color 0.3s ease;
}

.search-input:focus {
  outline: none;
  border-color: #ff6b35;
}

.search-results {
  margin-bottom: 3rem;
}

.search-results h3 {
  color: #ffffff;
  margin-bottom: 1.5rem;
  font-size: 1.5rem;
}

.search-results-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.search-result-card {
  background-color: #1a1a1a;
  border: 2px solid #333;
  border-radius: 12px;
  padding: 1.5rem;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  gap: 1.5rem;
}

.search-result-card:hover {
  border-color: #ff6b35;
  transform: translateY(-3px);
  box-shadow: 0 8px 16px rgba(255, 107, 53, 0.2);
}

.search-result-image {
  width: 100px;
  height: 150px;
  object-fit: cover;
  border-radius: 8px;
  flex-shrink: 0;
}

.search-result-info h4 {
  color: #ffffff;
  margin-bottom: 0.75rem;
  font-size: 1.1rem;
}

.search-result-info p {
  color: #cccccc;
  font-size: 0.95rem;
  line-height: 1.5;
  margin: 0;
}

.movie-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 2.5rem;
}

.movie-card {
  background-color: #1a1a1a;
  border-radius: 12px;
  overflow: hidden;
  transition: all 0.3s ease;
  animation: fadeIn 0.6s ease-out;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
}

.movie-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 12px 24px rgba(255, 107, 53, 0.3);
}

.movie-image-container {
  position: relative;
  height: 250px;
  overflow: hidden;
}

.movie-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.movie-card:hover .movie-image {
  transform: scale(1.08);
}

.remove-btn {
  position: absolute;
  top: 15px;
  right: 15px;
  background: rgba(0, 0, 0, 0.8);
  border: none;
  border-radius: 50%;
  width: 35px;
  height: 35px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.remove-btn:hover {
  background: rgba(255, 68, 68, 0.9);
  transform: scale(1.1);
}

.remove-icon {
  width: 18px;
  height: 18px;
  filter: invert(1);
}

.movie-info {
  padding: 2rem;
}

.movie-title {
  color: #ffffff;
  font-size: 1.4rem;
  font-weight: bold;
  margin-bottom: 1rem;
}

.movie-description {
  color: #cccccc;
  font-size: 1rem;
  line-height: 1.6;
  margin: 0;
}

.empty-state {
  text-align: center;
  padding: 4rem;
  color: #cccccc;
  font-size: 1.2rem;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Tablet Landscape Responsive */
@media (max-width: 1024px) {
  .movie-grid-section {
    padding: 4rem 0;
  }

  .section-header {
    margin-bottom: 3rem;
    gap: 2rem;
  }

  .section-title {
    font-size: 2.5rem;
    letter-spacing: 1.5px;
  }

  .search-container {
    max-width: 400px;
  }

  .movie-grid {
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 2rem;
  }

  .search-results-grid {
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  }

  .movie-image-container {
    height: 220px;
  }

  .movie-info {
    padding: 1.75rem;
  }

  .movie-title {
    font-size: 1.3rem;
  }
}

/* Tablet Portrait Responsive */
@media (max-width: 768px) {
  .movie-grid-section {
    padding: 3rem 0;
  }

  .section-header {
    flex-direction: column;
    align-items: stretch;
    gap: 1.5rem;
    margin-bottom: 2.5rem;
  }

  .section-title {
    font-size: 2rem;
    text-align: center;
    letter-spacing: 1px;
  }

  .search-container {
    max-width: 100%;
  }

  .search-input {
    padding: 12px 12px 12px 40px;
    font-size: 1rem;
  }

  .search-icon {
    left: 12px;
  }

  .movie-grid {
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
  }

  .search-results-grid {
    grid-template-columns: 1fr;
  }

  .movie-image-container {
    height: 200px;
  }

  .movie-info {
    padding: 1.5rem;
  }

  .movie-title {
    font-size: 1.25rem;
  }

  .search-result-card {
    padding: 1rem;
    gap: 1rem;
  }

  .search-result-image {
    width: 80px;
    height: 120px;
  }
}

/* Mobile Large Responsive */
@media (max-width: 480px) {
  .movie-grid-section {
    padding: 2rem 0;
  }

  .section-title {
    font-size: 1.75rem;
  }

  .movie-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .movie-image-container {
    height: 180px;
  }

  .movie-info {
    padding: 1rem;
  }

  .movie-title {
    font-size: 1.1rem;
  }

  .movie-description {
    font-size: 0.9rem;
  }

  .search-result-card {
    flex-direction: column;
    text-align: center;
  }

  .search-result-image {
    width: 100%;
    height: 150px;
    margin: 0 auto;
  }

  .empty-state {
    padding: 2rem;
    font-size: 1rem;
  }
}

/* Mobile Small Responsive */
@media (max-width: 360px) {
  .section-title {
    font-size: 1.5rem;
  }

  .search-input {
    padding: 10px 10px 10px 35px;
    font-size: 0.9rem;
  }

  .search-icon {
    left: 10px;
    width: 18px;
    height: 18px;
  }

  .movie-image-container {
    height: 160px;
  }

  .movie-info {
    padding: 0.75rem;
  }

  .movie-title {
    font-size: 1rem;
  }

  .movie-description {
    font-size: 0.85rem;
  }
}
</style> 
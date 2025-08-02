<template>
  <header class="header">
    <div class="container">
      <div class="header-content">
        <!-- Logo -->
        <div class="logo">
          <img src="/Assets/Logos/Logo White.svg" alt="Logoipsum" class="logo-img">
        </div>

        <!-- Desktop Navigation -->
        <nav class="nav-desktop" v-if="isDesktop && !isMobileMenuOpen">
          <ul class="nav-list">
            <li><a href="#" class="nav-link">HOME</a></li>
            <li><a href="#" class="nav-link">OUR SCREENS</a></li>
            <li><a href="#" class="nav-link">SCHEDULE</a></li>
            <li><a href="#" class="nav-link active">MOVIE LIBRARY</a></li>
            <li><a href="#" class="nav-link">LOCATION & CONTACT</a></li>
          </ul>
        </nav>
        <!-- Tablet Navigation -->
        <nav class="nav-tablet" v-else-if="isTablet && !isMobileMenuOpen">
          <ul class="nav-list">
            <li><a href="#" class="nav-link">HOME</a></li>
            <li><a href="#" class="nav-link">OUR SCREENS</a></li>
            <li><a href="#" class="nav-link active">MOVIE LIBRARY</a></li>
          </ul>
        </nav>

        <!-- Hamburger Menu Button (always visible, uses provided icon) -->
        <button 
          class="hamburger-btn" 
          @click="toggleMobileMenu"
          :class="{ 'active': isMobileMenuOpen }"
          aria-label="Toggle menu"
        >
          <img 
            v-if="!isMobileMenuOpen"
            src="../assets/Icons/Menu White.svg"
            alt="Open Menu"
            class="hamburger-icon"
          >
          <img 
            v-else
            src="../assets/Icons/Close White.svg" 
            alt="Close Menu"
            class="hamburger-icon"
          >
        </button>
      </div>

      <!-- Mobile Navigation Overlay -->
      <div 
        class="mobile-menu-overlay" 
        v-show="isMobileMenuOpen"
        @click="closeMobileMenu"
      ></div>
      
      <!-- Navigation Sidebar -->
      <nav class="nav-mobile" :class="{ 'open': isMobileMenuOpen }">
        <div class="mobile-menu-header">
          <button 
            class="mobile-close-btn"
            @click="closeMobileMenu"
            aria-label="Close menu"
          >
            <img src="../assets/Icons/Close White.svg"  alt="Close" class="close-icon">
          </button>
        </div>
        <ul class="nav-list-mobile">
          <li v-if="isMobile"><a href="#" class="nav-link-mobile" @click="closeMobileMenu">HOME</a></li>
          <li v-if="isMobile"><a href="#" class="nav-link-mobile" @click="closeMobileMenu">OUR SCREENS</a></li>
          <li v-if="isMobile"><a href="#" class="nav-link-mobile" @click="closeMobileMenu">SCHEDULE</a></li>
          <li v-if="isMobile || isTablet"><a href="#" class="nav-link-mobile active" @click="closeMobileMenu">MOVIE LIBRARY</a></li>
          <li v-if="isMobile || isTablet"><a href="#" class="nav-link-mobile" @click="closeMobileMenu">LOCATION & CONTACT</a></li>
          <li v-if="isMobile || isTablet || isDesktop"><a href="#" class="nav-link-mobile" @click="closeMobileMenu">GALLERY</a></li>
        </ul>
      </nav>
    </div>
  </header>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

const isMobileMenuOpen = ref(false)
const windowWidth = ref(window.innerWidth)

const handleResize = () => {
  windowWidth.value = window.innerWidth
}

onMounted(() => {
  window.addEventListener('resize', handleResize)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize)
})

const isMobile = computed(() => windowWidth.value <= 768)
const isTablet = computed(() => windowWidth.value > 768 && windowWidth.value <= 1024)
const isDesktop = computed(() => windowWidth.value > 1024)

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
  // Prevent body scroll when menu is open
  if (isMobileMenuOpen.value) {
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = ''
  }
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
  document.body.style.overflow = ''
}
</script>

<style scoped>
.header {
  background-color: #000000;
  padding: 1.25rem 0;
  position: sticky;
  font-family: var(--main-font);
  top: 0;
  z-index: 1000;
  border-bottom: 1px solid #333;
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  min-height: 56px;
}

.logo-img {
  height: 45px;
  width: auto;
}

.nav-desktop, .nav-tablet {
  display: block;
}

.nav-list {
  display: flex;
  list-style: none;
  gap: 2.5rem;
  margin: 0;
  padding: 0;
}

.nav-link {
  color: #ffffff;
  text-decoration: none;
  font-weight: bold;
  text-transform: uppercase;
  font-family: var(--main-font);
  font-size: 1rem;
  letter-spacing: 1.5px;
  transition: all 0.3s ease;
  position: relative;
  padding: 0.5rem 0;
}

.nav-link:hover {
   color: #ffffff;
  transform: translateY(-2px);
}

.nav-link.active {
  color: #ffffff;  
}

.nav-link.active::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 0;
  width: 100%;
  height: 3px;
  background-color: #ffffff;
}

.hamburger-btn {
  display: flex;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
  transition: all 0.3s ease;
  align-items: center;
  justify-content: center;
}

.hamburger-icon {
  width: 32px;
  height: 32px;
   filter: brightness(0) invert(1);
}

/* Mobile Menu Overlay */
.mobile-menu-overlay {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.6);
  z-index: 999;
  transition: opacity 0.3s;
}

/* Mobile Navigation Sidebar */
.nav-mobile {
  display: none;
  position: fixed;
  top: 0;
  right: -340px;
  width: 340px;
  height: 100vh;
  background-color: #000000;
  z-index: 1001;
  transition: right 0.3s cubic-bezier(.4,0,.2,1);
  box-shadow: -2px 0 10px rgba(0, 0, 0, 0.3);
  padding-top: 0;
}

.nav-mobile.open {
  right: 0;
}

.mobile-menu-header {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding: 1.25rem 1.5rem 0.5rem 1.5rem;
  border-bottom: 1px solid #333;
}

.mobile-close-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
  align-items: center;
  justify-content: center;
}

.close-icon {
  width: 28px;
  height: 28px;
 filter: brightness(0) invert(1);
}

.nav-list-mobile {
  list-style: none;
  margin: 0;
  padding: 1.5rem 0.5rem 0.5rem 0.5rem;
}

.nav-link-mobile {
  display: block;
  color: #ffffff;
  text-decoration: none;
  font-weight: bold;
  text-transform: uppercase;
  font-family: var(--main-font);
  font-size: 1.1rem;
  letter-spacing: 1.5px;
  padding: 1.25rem 1.5rem 1.25rem 2rem;
  transition: all 0.3s ease;
  border-bottom: 1px solid #333;
  border-radius: 4px;
}

.nav-link-mobile:hover,
.nav-link-mobile.active {
    color: #ffffff;
  background-color: #1a1a1a;
  padding-left: 2.5rem;
}

/* Desktop only: hide hamburger if not needed */
@media (min-width: 1025px) {
  .nav-desktop {
    display: block;
  }
  .nav-tablet {
    display: none !important;
  }
  .nav-mobile {
    display: block;
    width: 340px;
    right: -340px;
  }
  .hamburger-btn {
    display: flex;
  }
}

/* Tablet Responsive */
@media (max-width: 1024px) and (min-width: 769px) {
  .nav-desktop {
    display: none !important;
  }
  .nav-tablet {
    display: block;
  }
  .nav-mobile {
    display: block;
    width: 320px;
    right: -320px;
  }
  .hamburger-btn {
    display: flex;
  }
}

/* Mobile Responsive */
@media (max-width: 768px) {
  .logo-img {
    height: 35px;
  }
  .nav-desktop, .nav-tablet {
    display: none !important;
  }
  .nav-mobile {
    width: 260px;
    right: -260px;
    display: block;
  }
  .nav-link-mobile {
    font-size: 0.95rem;
    padding: 0.9rem 1rem 0.9rem 1.5rem;
  }
  .mobile-menu-header {
    padding: 0.75rem 1rem 0.5rem 1rem;
  }
  .close-icon {
    width: 22px;
    height: 22px;
  }
}
@media (max-width: 480px) {
  .header {
    padding: 0.75rem 0;
  }
  .logo-img {
    height: 30px;
  }
  .nav-mobile {
    width: 200px;
    right: -200px;
  }
  .nav-link-mobile {
    font-size: 0.85rem;
    padding: 0.7rem 0.7rem 0.7rem 1.2rem;
  }
  .mobile-menu-header {
    padding: 0.5rem 0.7rem 0.5rem 0.7rem;
  }
  .close-icon {
    width: 18px;
    height: 18px;
  }
}
@media (max-width: 360px) {
  .nav-mobile {
    width: 100vw;
    right: -100vw;
  }
}
</style> 
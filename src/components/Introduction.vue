<template>
  <section class="introduction">
    <div class="container">
      <div class="intro-content">
        <h2 class="intro-title">MOVIE LIBRARY</h2>
        <p class="intro-description">
          Lorem ipsum dolor sit amet, consetetur sadipscing elitr,
          sed diam nonumy eirmod tempor invidunt ut labore et
          dolore magna aliquyam erat, sed diam voluptua.
        </p>
      </div>
    </div>
  </section>
</template>

<script setup>
// No additional logic needed for this component
</script>

<style scoped>
.introduction {
  padding: 4rem 0;
  background-color: #000000;
}

.container {
  width: 100%;
  max-width: 1200px; /* Adjust this as needed */
  margin: 1; /* Remove auto margin to align left */
  padding: 0 2rem; /* Add some padding to prevent content touching edges */
}

.intro-content {
  text-align: left;
  max-width: 900px;
  margin: 1; /* Remove auto margin to align left */
}

.intro-title {
  font-size: 3.5rem;
  font-weight: bold;
  color: #ffffff;
  margin-bottom: 2.5rem;
  text-transform: uppercase;
  letter-spacing: 3px;
  animation: fadeIn 0.8s ease-out;
}

.intro-description {
  font-size: 1.3rem;
  color: #cccccc;
  line-height: 1.8;
  margin-bottom: 0;
  animation: fadeIn 0.8s ease-out 0.2s both;
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

/* Tablet Responsive */
@media (max-width: 1024px) {
  .introduction {
    padding: 4rem 0;
  }
  
  .container {
    padding: 0 1.5rem;
  }

  .intro-title {
    font-size: 3rem;
    letter-spacing: 2px;
    margin-bottom: 2rem;
  }

  .intro-description {
    font-size: 1.2rem;
    line-height: 1.7;
  }
}

/* Mobile Responsive */
@media (max-width: 768px) {
  .introduction {
    padding: 3rem 0;
  }
  
  .container {
    padding: 0 1rem;
  }

  .intro-title {
    font-size: 2.5rem;
    letter-spacing: 1.5px;
    margin-bottom: 1.5rem;
  }

  .intro-description {
    font-size: 1.1rem;
    line-height: 1.6;
  }
}

@media (max-width: 480px) {
  .introduction {
    padding: 2.5rem 0;
  }
  
  .container {
    padding: 0 1rem;
  }

  .intro-title {
    font-size: 2rem;
    letter-spacing: 1px;
    margin-bottom: 1.25rem;
  }

  .intro-description {
    font-size: 1rem;
    line-height: 1.5;
  }
}

@media (max-width: 360px) {
  .introduction {
    padding: 2rem 0;
  }
  
  .container {
    padding: 0 0.75rem;
  }

  .intro-title {
    font-size: 1.75rem;
    letter-spacing: 0.5px;
  }

  .intro-description {
    font-size: 0.9rem;
  }
}
</style>

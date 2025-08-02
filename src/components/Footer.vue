<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <!-- Company Information -->
        <div class="company-info">
          <h3 class="company-name">Amadeus IT Group</h3>
          <div class="address">
            <p>C. Salvador de Madariaga, 1</p>
            <p>28027 Madrid</p>
            <p>Spain</p>
          </div>
          <p class="copyright">
            Copyright © {{ currentYear }} Amadeus Hotels. All rights reserved.
          </p>
        </div>

        <!-- Social Media -->
        <div class="social-section">
          <p class="follow-text">Follow us on</p>
          <div class="social-icons">
            <a href="#" class="social-link" title="Follow us on Twitter">
              <img src="/Assets/Icons/Twitter White.svg" alt="Twitter" class="social-icon">
            </a>
            <a href="#" class="social-link" title="Follow us on YouTube">
              <img src="/Assets/Icons/Youtube White.svg" alt="YouTube" class="social-icon">
            </a>
          </div>
          <div class="legal-links">
            <a href="#" class="legal-link">Privacy Policy</a>
            <span class="separator">|</span>
            <a href="#" class="legal-link">Cookie Policy</a>
            <span class="separator">|</span>
            <a href="#" class="legal-link">Legal Notice</a>
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { computed } from 'vue'

const currentYear = computed(() => {
  return new Date().getFullYear()
})
</script>

<style scoped>
.footer {
  background-color: #1a1a1a;
  padding: 4rem 0 2.5rem;
  border-top: 1px solid #333;
}

.footer-content {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  flex-wrap: wrap;
  gap: 3rem;
}

.company-info {
  flex: 1;
  min-width: 350px;
}

.company-name {
  color: #ffffff;
  font-size: 1.8rem;
  font-weight: bold;
  margin-bottom: 1.5rem;
}

.address {
  margin-bottom: 2rem;
}

.address p {
  color: #cccccc;
  margin-bottom: 0.5rem;
  font-size: 1.1rem;
  line-height: 1.5;
}

.copyright {
  color: #888888;
  font-size: 1rem;
  margin: 0;
}

.social-section {
  text-align: right;
  min-width: 250px;
}

.follow-text {
  color: #ffffff;
  font-weight: bold;
  margin-bottom: 1.5rem;
  font-size: 1.1rem;
}

.social-icons {
  display: flex;
  gap: 1.5rem;
  margin-bottom: 2rem;
  justify-content: flex-end;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 50px;
  height: 50px;
  background-color: #333;
  border-radius: 50%;
  transition: all 0.3s ease;
  text-decoration: none;
}

.social-link:hover {
  background-color: #ff6b35;
  transform: translateY(-3px);
  box-shadow: 0 6px 12px rgba(255, 107, 53, 0.3);
}

.social-icon {
  width: 24px;
  height: 24px;
  filter: invert(1);
}

.legal-links {
  display: flex;
  gap: 1rem;
  justify-content: flex-end;
  flex-wrap: wrap;
}

.legal-link {
  color: #888888;
  text-decoration: none;
  font-size: 0.9rem;
  transition: color 0.3s ease;
}

.legal-link:hover {
  color: #ff6b35;
}

.separator {
  color: #888888;
  font-size: 0.9rem;
}

/* Tablet Responsive */
@media (max-width: 1024px) {
  .footer {
    padding: 3.5rem 0 2rem;
  }

  .footer-content {
    gap: 2.5rem;
  }

  .company-info {
    min-width: 300px;
  }

  .company-name {
    font-size: 1.6rem;
  }

  .address p {
    font-size: 1rem;
  }

  .social-section {
    min-width: 220px;
  }

  .social-link {
    width: 45px;
    height: 45px;
  }

  .social-icon {
    width: 22px;
    height: 22px;
  }
}

/* Mobile Responsive */
@media (max-width: 768px) {
  .footer {
    padding: 3rem 0 1.5rem;
  }

  .footer-content {
    flex-direction: column;
    align-items: center;
    text-align: center;
    gap: 2.5rem;
  }

  .company-info {
    min-width: auto;
    width: 100%;
  }

  .company-name {
    font-size: 1.4rem;
    margin-bottom: 1.25rem;
  }

  .address {
    margin-bottom: 1.5rem;
  }

  .address p {
    font-size: 0.95rem;
  }

  .copyright {
    font-size: 0.9rem;
  }

  .social-section {
    text-align: center;
    min-width: auto;
    width: 100%;
  }

  .follow-text {
    font-size: 1rem;
    margin-bottom: 1.25rem;
  }

  .social-icons {
    justify-content: center;
    margin-bottom: 1.5rem;
  }

  .legal-links {
    justify-content: center;
  }

  .legal-link {
    font-size: 0.85rem;
  }

  .separator {
    font-size: 0.85rem;
  }
}

@media (max-width: 480px) {
  .footer {
    padding: 2.5rem 0 1rem;
  }

  .footer-content {
    gap: 2rem;
  }

  .company-name {
    font-size: 1.25rem;
    margin-bottom: 1rem;
  }

  .address {
    margin-bottom: 1.25rem;
  }

  .address p {
    font-size: 0.9rem;
  }

  .copyright {
    font-size: 0.85rem;
  }

  .follow-text {
    font-size: 0.95rem;
    margin-bottom: 1rem;
  }

  .social-link {
    width: 40px;
    height: 40px;
  }

  .social-icon {
    width: 20px;
    height: 20px;
  }

  .legal-links {
    gap: 0.75rem;
  }

  .legal-link {
    font-size: 0.8rem;
  }

  .separator {
    font-size: 0.8rem;
  }
}

@media (max-width: 360px) {
  .footer {
    padding: 2rem 0 1rem;
  }

  .footer-content {
    gap: 1.5rem;
  }

  .company-name {
    font-size: 1.1rem;
  }

  .address p {
    font-size: 0.85rem;
  }

  .copyright {
    font-size: 0.8rem;
  }

  .follow-text {
    font-size: 0.9rem;
  }

  .social-link {
    width: 35px;
    height: 35px;
  }

  .social-icon {
    width: 18px;
    height: 18px;
  }

  .legal-links {
    gap: 0.5rem;
  }

  .legal-link {
    font-size: 0.75rem;
  }

  .separator {
    font-size: 0.75rem;
  }
}
</style> 
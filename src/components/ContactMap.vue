<template>
  <section class="contact-map-section">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">How to reach us</h2>
        <p class="section-description">
          Get in touch with us for any inquiries or feedback. We'd love to hear from you!
        </p>
      </div>

      <div class="contact-map-content">
        <!-- Contact Form -->
        <div class="contact-form-container">
          <form @submit.prevent="handleSubmit" class="contact-form">
            <div class="form-row">
              <div class="form-group">
                <label for="firstName">First Name *</label>
                <input 
                  type="text" 
                  id="firstName"
                  v-model="formData.firstName"
                  @blur="validateField('firstName')"
                  :class="{ 'error': errors.firstName }"
                  class="form-control"
                >
                <span v-if="errors.firstName" class="error-message">{{ errors.firstName }}</span>
              </div>
              <div class="form-group">
                <label for="lastName">Last Name *</label>
                <input 
                  type="text" 
                  id="lastName"
                  v-model="formData.lastName"
                  @blur="validateField('lastName')"
                  :class="{ 'error': errors.lastName }"
                  class="form-control"
                >
                <span v-if="errors.lastName" class="error-message">{{ errors.lastName }}</span>
              </div>
            </div>

            <div class="form-row">
              <div class="form-group">
                <label for="email">Email *</label>
                <input 
                  type="email" 
                  id="email"
                  v-model="formData.email"
                  @blur="validateField('email')"
                  :class="{ 'error': errors.email }"
                  class="form-control"
                >
                <span v-if="errors.email" class="error-message">{{ errors.email }}</span>
              </div>
              <div class="form-group">
                <label for="telephone">Telephone</label>
                <input 
                  type="tel" 
                  id="telephone"
                  v-model="formData.telephone"
                  class="form-control"
                >
              </div>
            </div>

            <div class="form-group">
              <label for="message">Message *</label>
              <textarea 
                id="message"
                v-model="formData.message"
                @blur="validateField('message')"
                :class="{ 'error': errors.message }"
                class="form-control"
                rows="6"
              ></textarea>
              <span v-if="errors.message" class="error-message">{{ errors.message }}</span>
            </div>

            <p class="required-note">*required fields</p>

            <div class="form-group checkbox-group">
              <label class="checkbox-label">
                <input 
                  type="checkbox" 
                  v-model="formData.agreeToTerms"
                  class="checkbox-input"
                  required
                >
                <span class="checkmark"></span>
                I agree to the <a href="#" class="terms-link">Terms & Conditions</a>
              </label>
            </div>

            <button type="submit" class="btn btn-primary submit-btn">
              SUBMIT
            </button>
          </form>
        </div>

        <!-- Map Container -->
        <div class="map-container">
          <iframe 
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3037.1234567890123!2d-3.7037902!3d40.4167754!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x0%3A0x0!2zNDDCsDI1JzAwLjQiTiAzwrA0MicxMy42Ilc!5e0!3m2!1sen!2ses!4v1234567890123"
            width="100%" 
            height="500" 
            style="border:0;" 
            allowfullscreen="" 
            loading="lazy" 
            referrerpolicy="no-referrer-when-downgrade"
            class="map-iframe"
          ></iframe>
          <div class="map-overlay">
            <div class="map-pin">
              <img src="../assets/Icons/Map Pin Gold.svg" alt="Location" class="pin-icon">
              <span class="pin-text">Amadeus HQ</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, reactive } from 'vue'

const formData = reactive({
  firstName: '',
  lastName: '',
  email: '',
  telephone: '',
  message: '',
  agreeToTerms: false
})

const errors = reactive({
  firstName: '',
  lastName: '',
  email: '',
  message: ''
})

const validateField = (fieldName) => {
  errors[fieldName] = ''
  
  switch (fieldName) {
    case 'firstName':
      if (!formData.firstName.trim()) {
        errors.firstName = 'First name is required'
      } else if (formData.firstName.trim().length < 2) {
        errors.firstName = 'First name must be at least 2 characters'
      }
      break
      
    case 'lastName':
      if (!formData.lastName.trim()) {
        errors.lastName = 'Last name is required'
      } else if (formData.lastName.trim().length < 2) {
        errors.lastName = 'Last name must be at least 2 characters'
      }
      break
      
    case 'email':
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
      if (!formData.email.trim()) {
        errors.email = 'Email is required'
      } else if (!emailRegex.test(formData.email)) {
        errors.email = 'Please enter a valid email address'
      }
      break
      
    case 'message':
      if (!formData.message.trim()) {
        errors.message = 'Message is required'
      } else if (formData.message.trim().length < 10) {
        errors.message = 'Message must be at least 10 characters'
      }
      break
  }
}

const validateForm = () => {
  validateField('firstName')
  validateField('lastName')
  validateField('email')
  validateField('message')
  
  return !errors.firstName && !errors.lastName && !errors.email && !errors.message
}

const handleSubmit = () => {
  if (validateForm()) {
    if (!formData.agreeToTerms) {
      alert('Please agree to the Terms & Conditions')
      return
    }
    
    // Show success message with form data
    const formDataString = `
Form submitted successfully!

First Name: ${formData.firstName}
Last Name: ${formData.lastName}
Email: ${formData.email}
Telephone: ${formData.telephone || 'Not provided'}
Message: ${formData.message}
Agreed to Terms: ${formData.agreeToTerms ? 'Yes' : 'No'}
    `
    
    alert(formDataString)
    
    // Reset form
    Object.keys(formData).forEach(key => {
      if (key === 'agreeToTerms') {
        formData[key] = false
      } else {
        formData[key] = ''
      }
    })
    
    // Clear errors
    Object.keys(errors).forEach(key => {
      errors[key] = ''
    })
  }
}
</script>


<style scoped>
.contact-map-section {
  padding: 5rem 0;
  background-color: #000000;
}

.section-header {
  text-align: center;
  margin-bottom: 4rem;
}

.section-title {
  font-size: 3rem;
  font-weight: bold;
  color: #ffffff;
  margin-bottom: 1.5rem;
  text-transform: uppercase;
  letter-spacing: 2px;
}

.section-description {
  font-size: 1.2rem;
  color: #cccccc;
  max-width: 700px;
  margin: 0 auto;
  line-height: 1.6;
}

.contact-map-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: start;
}

.contact-form-container {
  background-color: #1a1a1a;
  padding: 3rem;
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
}

.contact-form {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group label {
  color: #ffffff;
  font-weight: bold;
  margin-bottom: 0.75rem;
  font-size: 1rem;
}

.form-control {
  padding: 15px;
  border: 2px solid #333;
  border-radius: 8px;
  background-color: #2a2a2a;
  color: white;
  font-size: 1rem;
  transition: border-color 0.3s ease;
  resize: vertical;
}

.form-control:focus {
  outline: none;
  border-color: #ff6b35;
}

.form-control.error {
  border-color: #ff4444;
}

.error-message {
  color: #ff4444;
  font-size: 0.875rem;
  margin-top: 0.5rem;
}

.required-note {
  color: #888888;
  font-size: 0.9rem;
  margin: 0;
}

.checkbox-group {
  flex-direction: row;
  align-items: center;
  gap: 0.75rem;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  color: #ffffff;
  font-size: 1rem;
}

.checkbox-input {
  width: 20px;
  height: 20px;
  accent-color: #ff6b35;
}

.terms-link {
  color: #ff6b35;
  text-decoration: none;
  transition: color 0.3s ease;
}

.terms-link:hover {
  color: #e55a2b;
  text-decoration: underline;
}

.submit-btn {
  background-color: #ff6b35;
  color: white;
  padding: 18px 36px;
  font-size: 1.2rem;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 1px;
  transition: all 0.3s ease;
  border-radius: 8px;
}

.submit-btn:hover {
  background-color: #e55a2b;
  transform: translateY(-3px);
  box-shadow: 0 8px 16px rgba(255, 107, 53, 0.3);
}

.map-container {
  position: relative;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
}

.map-iframe {
  width: 100%;
  height: 500px;
  border: none;
}

.map-overlay {
  position: absolute;
  top: 25px;
  left: 25px;
  z-index: 10;
}

.map-pin {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  background: rgba(0, 0, 0, 0.9);
  padding: 0.75rem 1.25rem;
  border-radius: 25px;
  color: white;
  font-weight: bold;
  font-size: 1rem;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
}

.pin-icon {
  width: 24px;
  height: 24px;
  filter: invert(1);
}

/* Tablet Landscape Responsive */
@media (max-width: 1024px) {
  .contact-map-section {
    padding: 4rem 0;
  }

  .section-title {
    font-size: 2.5rem;
    letter-spacing: 1.5px;
  }

  .contact-map-content {
    gap: 3rem;
  }

  .contact-form-container {
    padding: 2.5rem;
  }

  .map-iframe {
    height: 450px;
  }

  .form-row {
    gap: 1.25rem;
  }

  .contact-form {
    gap: 1.75rem;
  }
}

/* Tablet Portrait Responsive */
@media (max-width: 768px) {
  /* ... (keep existing tablet styles) ... */

  .contact-map-content {
    grid-template-columns: 1fr; /* Single column layout */
    gap: 2.5rem;
  }

  /* Ensure map container comes after form */
  .map-container {
    order: 2; /* This will make map appear after form */
  }

  .contact-form-container {
    order: 1; /* This will make form appear first */
  }
}

/* Mobile Large Responsive */
@media (max-width: 480px) {
  .contact-map-section {
    padding: 2rem 0;
  }

  .section-title {
    font-size: 1.75rem;
  }

  .section-description {
    font-size: 1rem;
  }

  .contact-form-container {
    padding: 1.5rem;
  }

  .contact-form {
    gap: 1.25rem;
  }

  .form-group label {
    font-size: 0.95rem;
  }

  .form-control {
    padding: 10px;
    font-size: 0.9rem;
  }

  .submit-btn {
    padding: 12px 24px;
    font-size: 1rem;
  }

  .map-iframe {
    height: 300px;
  }

  .map-overlay {
    top: 15px;
    left: 15px;
  }

  .map-pin {
    padding: 0.4rem 0.8rem;
    font-size: 0.85rem;
  }

  .pin-icon {
    width: 18px;
    height: 18px;
  }

  .checkbox-label {
    font-size: 0.9rem;
  }

  .checkbox-input {
    width: 18px;
    height: 18px;
  }
}

/* Mobile Small Responsive */
@media (max-width: 360px) {
  .contact-form-container {
    padding: 1rem;
  }

  .contact-form {
    gap: 1rem;
  }

  .form-control {
    padding: 8px;
    font-size: 0.85rem;
  }

  .submit-btn {
    padding: 10px 20px;
    font-size: 0.9rem;
  }

  .map-iframe {
    height: 250px;
  }

  .map-pin {
    padding: 0.3rem 0.6rem;
    font-size: 0.8rem;
  }

  .pin-icon {
    width: 16px;
    height: 16px;
  }
}
</style> 
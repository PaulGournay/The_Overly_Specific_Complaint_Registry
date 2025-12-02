<template>
  <div class="contact-container">
    <div class="content-width">
      <div class="page-header">
        <h1>Contact Us</h1>
        <p>Have a question or feedback? We'd love to hear from you.</p>
      </div>

      <div class="grid-layout">
        <div class="ios-card form-section">
          <div class="card-header">
            <h2>Send a Message to the Admin</h2>
            <p class="subtitle">
              Direct line to the Archivist (Bug reports, features, etc.)
            </p>
          </div>

          <form @submit.prevent="submitForm" class="ios-form">
            <div class="form-group">
              <label>Name</label>
              <input
                v-model="form.name"
                type="text"
                placeholder="Your name"
                class="ios-input"
                required
              />
            </div>

            <div class="form-group">
              <label>Email</label>
              <input
                v-model="form.email"
                type="email"
                placeholder="your.email@example.com"
                class="ios-input"
                required
              />
            </div>

            <div class="form-group">
              <label>Subject</label>
              <input
                v-model="form.subject"
                type="text"
                placeholder="What's this about?"
                class="ios-input"
                required
              />
            </div>

            <div class="form-group">
              <label>Message</label>
              <textarea
                v-model="form.message"
                placeholder="Tell us your thoughts..."
                class="ios-input textarea"
                rows="6"
                required
              ></textarea>
            </div>

            <button type="submit" class="ios-btn-primary">Send Message</button>
          </form>

          <transition name="fade">
            <p
              v-if="submitMessage"
              class="status-msg"
              :class="{ error: submitMessage.includes('Error') }"
            >
              {{ submitMessage }}
            </p>
          </transition>
        </div>

        <div class="side-content">
          <div class="ios-card info-card">
            <h2>Other Ways to Reach Us</h2>

            <div class="info-item">
              <div class="icon-box blue">✉️</div>
              <div class="info-text">
                <h3>Email</h3>
                <p>contact@complaintregistry.com</p>
              </div>
            </div>

            <div class="separator"></div>

            <div class="info-item">
              <div class="icon-box green">📱</div>
              <div class="info-text">
                <h3>Social Media</h3>
                <p>@ComplaintRegistry</p>
              </div>
            </div>

            <div class="separator"></div>

            <div class="info-item">
              <div class="icon-box gray">⏱️</div>
              <div class="info-text">
                <h3>Response Time</h3>
                <p>Within 24-48 hours</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const form = ref({
  name: "",
  email: "",
  subject: "",
  message: "",
});

const submitMessage = ref("");

const submitForm = async () => {
  submitMessage.value = "Sending...";

  try {
    await axios.post("http://localhost:3000/api/contact", form.value);

    submitMessage.value = "Thank you! Your message has been sent to the Admin.";

    // Reset form
    form.value = {
      name: "",
      email: "",
      subject: "",
      message: "",
    };
  } catch (error) {
    console.error(error);
    submitMessage.value = "Error sending message. Please try again.";
  }
};
</script>

<style scoped>
/* --- Layout & Background --- */
.contact-container {
  background-color: #f2f2f7; /* iOS Grouped Background */
  min-height: 100vh;
  padding: 40px 20px 80px;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial,
    sans-serif;
}

.content-width {
  max-width: 1000px;
  margin: 0 auto;
}

/* --- Typography --- */
.page-header {
  text-align: center;
  margin-bottom: 40px;
}

.page-header h1 {
  font-size: 40px;
  font-weight: 800;
  color: #1c1c1e;
  margin: 0 0 10px 0;
  letter-spacing: -1px;
}

.page-header p {
  font-size: 18px;
  color: #8e8e93;
  margin: 0;
}

/* --- Grid Layout --- */
.grid-layout {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 24px;
  align-items: start;
}

/* --- iOS Cards --- */
.ios-card {
  background: #ffffff;
  border-radius: 20px;
  padding: 24px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.04);
}

.card-header {
  margin-bottom: 24px;
}

.card-header h2 {
  font-size: 24px;
  font-weight: 700;
  margin: 0 0 4px 0;
  color: #1c1c1e;
}

.subtitle {
  font-size: 14px;
  color: #8e8e93;
  margin: 0;
}

/* --- Forms & Inputs --- */
.ios-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-group label {
  font-size: 14px;
  font-weight: 600;
  color: #3a3a3c;
  margin-left: 4px;
}

.ios-input {
  background-color: #f2f2f7;
  border: 1px solid transparent;
  border-radius: 12px;
  padding: 14px 16px;
  font-size: 16px;
  color: #1c1c1e;
  font-family: inherit;
  transition: all 0.2s ease;
}

.ios-input:focus {
  background-color: #fff;
  border-color: #007aff;
  outline: none;
  box-shadow: 0 0 0 4px rgba(0, 122, 255, 0.1);
}

.ios-input.textarea {
  resize: vertical;
  min-height: 120px;
}

/* --- Buttons --- */
.ios-btn-primary {
  background-color: #007aff;
  color: white;
  border: none;
  padding: 14px 20px;
  border-radius: 12px;
  font-size: 17px;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.1s, background-color 0.2s;
  margin-top: 10px;
}

.ios-btn-primary:hover {
  background-color: #005ecb;
}

.ios-btn-primary:active {
  transform: scale(0.98);
}

/* --- Side Info Card --- */
.info-card h2 {
  font-size: 18px;
  font-weight: 700;
  color: #8e8e93;
  text-transform: uppercase;
  margin: 0 0 20px 0;
  letter-spacing: 0.5px;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 0;
}

.icon-box {
  width: 36px;
  height: 36px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  flex-shrink: 0;
}
.icon-box.blue {
  background-color: #eaf2ff;
}
.icon-box.green {
  background-color: #e5f9ea;
}
.icon-box.gray {
  background-color: #f2f2f7;
}

.info-text h3 {
  font-size: 16px;
  font-weight: 600;
  margin: 0 0 2px 0;
  color: #1c1c1e;
}

.info-text p {
  font-size: 14px;
  color: #8e8e93;
  margin: 0;
}

.separator {
  height: 1px;
  background-color: #e5e5ea;
  margin: 12px 0 12px 48px; /* Inset separator */
}

/* --- Messages --- */
.status-msg {
  text-align: center;
  margin-top: 15px;
  color: #34c759; /* Success Green */
  font-weight: 600;
  font-size: 15px;
}

.status-msg.error {
  color: #ff3b30; /* Error Red */
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* --- Responsive --- */
@media (max-width: 768px) {
  .grid-layout {
    grid-template-columns: 1fr;
  }

  .page-header h1 {
    font-size: 32px;
  }
}
</style>

<template>
  <div class="admin-messages-container">
    <div class="header-section">
      <h2>Inbox (Site Feedback)</h2>
      <p class="subtitle">Messages submitted via Contact Page</p>
    </div>

    <div v-if="loading" class="loading-state">Loading messages...</div>

    <div v-else-if="messages.length === 0" class="empty-state">
      <p>No new messages.</p>
    </div>

    <div v-else class="messages-list">
      <div v-for="msg in messages" :key="msg.id" class="ios-card message-card">
        <div class="msg-header">
          <div class="sender-info">
            <span class="sender-name">{{ msg.name }}</span>
            <span class="sender-email">&lt;{{ msg.email }}&gt;</span>
          </div>
          <span class="msg-date">{{ formatDate(msg.created_at) }}</span>
        </div>

        <div class="msg-body">
          <h4 class="msg-subject">{{ msg.subject }}</h4>
          <p class="msg-content">{{ msg.message }}</p>
        </div>

        <div class="msg-footer">
          <button @click="deleteMessage(msg.id)" class="ios-btn-destructive">
            Delete Message
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "AdminMessages",
  props: ["token"],
  data() {
    return {
      messages: [],
      loading: true,
    };
  },
  methods: {
    async fetchMessages() {
      try {
        const response = await axios.get("http://localhost:3000/api/admin/messages", {
          headers: { Authorization: `Bearer ${this.token}` },
        });
        this.messages = response.data;
      } catch (error) {
        console.error("Failed to fetch messages", error);
        alert("Access Denied or Server Error");
      } finally {
        this.loading = false;
      }
    },
    async deleteMessage(id) {
      if (!confirm("Are you sure you want to delete this message?")) return;
      try {
        await axios.delete(`http://localhost:3000/api/admin/messages/${id}`, {
          headers: { Authorization: `Bearer ${this.token}` },
        });
        // Remove from local list
        this.messages = this.messages.filter((m) => m.id !== id);
      } catch (error) {
        alert("Failed to delete message");
      }
    },
    formatDate(dateString) {
      const options = {
        year: "numeric",
        month: "short",
        day: "numeric",
        hour: "2-digit",
        minute: "2-digit",
      };
      return new Date(dateString).toLocaleDateString(undefined, options);
    },
  },
  mounted() {
    this.fetchMessages();
  },
};
</script>

<style scoped>
.admin-messages-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  padding-bottom: 60px;
}

.header-section {
  text-align: center;
  margin-bottom: 30px;
}

h2 {
  font-size: 28px;
  margin-bottom: 5px;
  color: #1c1c1e;
}

.subtitle {
  color: #8e8e93;
  margin: 0;
}

/* iOS Card Style */
.ios-card {
  background: white;
  border-radius: 16px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  border: 1px solid rgba(0, 0, 0, 0.02);
}

.msg-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 12px;
  border-bottom: 1px solid #f2f2f7;
  padding-bottom: 12px;
}

.sender-info {
  display: flex;
  flex-direction: column;
}

.sender-name {
  font-weight: 700;
  font-size: 16px;
  color: #1c1c1e;
}

.sender-email {
  font-size: 13px;
  color: #007aff;
}

.msg-date {
  font-size: 12px;
  color: #8e8e93;
}

.msg-body {
  margin-bottom: 20px;
}

.msg-subject {
  margin: 0 0 8px 0;
  font-size: 17px;
  font-weight: 600;
}

.msg-content {
  color: #3a3a3c;
  line-height: 1.5;
  white-space: pre-wrap; /* Preserves line breaks */
}

.msg-footer {
  display: flex;
  justify-content: flex-end;
}

.ios-btn-destructive {
  background-color: #ff3b30; /* System Red */
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  font-size: 13px;
  transition: opacity 0.2s;
}

.ios-btn-destructive:hover {
  opacity: 0.8;
}

.empty-state,
.loading-state {
  text-align: center;
  color: #8e8e93;
  margin-top: 40px;
}
</style>

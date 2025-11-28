<template>
  <div class="complaint-list">
    <div class="search-and-form">
      <input v-model="searchQuery" placeholder="Search complaints..." />

      <div v-if="user.role === 'archivist'" class="list_user">
        <div v-for="person in users" :key="person.id">
          <img :src="getImgUrl(person.pfp)" alt="PFP" />
          <p>{{ person.username }}</p>
          <button @click="banUser(person.id)">BAN</button>
        </div>
      </div>

      <div v-if="user.role === 'complainer'" class="new-complaint-form">
        <h3>Submit a New Overly Specific Complaint</h3>
        <form @submit.prevent="submitComplaint">
          <input v-model="newComplaint.title" placeholder="Complaint Title" required />
          <textarea
            v-model="newComplaint.detail"
            placeholder="Be extremely specific..."
            required
          ></textarea>
          <select v-model="newComplaint.category_id" required>
            <option disabled value="">Select a Category</option>
            <option v-for="cat in categories" :key="cat.id" :value="cat.id">
              {{ cat.name }}
            </option>
          </select>
          <button type="submit">Register Complaint</button>
        </form>
      </div>
    </div>

    <h2>The Registry (Sorted by Specificity Score)</h2>

    <div
      v-for="complaint in filteredComplaints"
      :key="complaint.id"
      class="complaint-card"
    >
      <div class="card-header">
        <strong>{{ complaint.title }}</strong>
        <span class="category-tag">{{ complaint.category_name }}</span>
      </div>
      <p>{{ complaint.detail }}</p>

      <div class="card-footer">
        <span class="score">Specificity Score: {{ complaint.specificity_score }}</span>

        <button v-if="user.role === 'complainer'" @click="upvoteComplaint(complaint)">
          Upvote (+1 Specificity)
        </button>

        <button
          v-if="user.id === complaint.complainer_id && !editingComplaint"
          @click="startEdit(complaint)"
        >
          Make More Specific (Edit)
        </button>

        <button
          v-if="user.role === 'archivist'"
          @click="deleteComplaint(complaint.id)"
          class="delete-btn"
        >
          Archive (Delete)
        </button>
      </div>

      <div
        v-if="editingComplaint && editingComplaint.id === complaint.id"
        class="edit-form"
      >
        <h4>Edit Complaint</h4>
        <input v-model="editingComplaint.title" placeholder="New Title" required />
        <textarea
          v-model="editingComplaint.detail"
          placeholder="New Detail"
          required
        ></textarea>
        <select v-model="editingComplaint.category_id" required>
          <option v-for="cat in categories" :key="cat.id" :value="cat.id">
            {{ cat.name }}
          </option>
        </select>
        <button @click="updateComplaint">Save</button>
        <button @click="cancelEdit">Cancel</button>
      </div>
    </div>

    <p v-if="!filteredComplaints.length">No complaints found matching your search.</p>
  </div>
</template>

<script>
import axios from "axios";

import pfp1 from "@/assets/pfp_image/pfp1.jpeg";
import pfp2 from "@/assets/pfp_image/pfp2.jpeg";
import pfp3 from "@/assets/pfp_image/pfp3.jpeg";
import pfp4 from "@/assets/pfp_image/pfp4.jpeg";

export default {
  name: "ComplaintList",
  props: ["user", "token"],
  data() {
    return {
      complaints: [],
      categories: [],
      users: [],
      pfpMap: {
        "pfp1.jpeg": pfp1,
        "pfp2.jpeg": pfp2,
        "pfp3.jpeg": pfp3,
        "pfp4.jpeg": pfp4,
      },
      newComplaint: {
        title: "",
        detail: "",
        category_id: "",
      },
      editingComplaint: null, // Stores the complaint being edited
      searchQuery: "",
    };
  },
  computed: {
    filteredComplaints() {
      // Filter by search query (case-insensitive on title/detail)
      const filtered = this.complaints.filter(
        (c) =>
          c.title.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          c.detail.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
      // Sorting is already done server-side by specificity_score DESC, but we'll re-sort here just in case of local updates
      return filtered.sort((a, b) => b.specificity_score - a.specificity_score);
    },
    // Axios instance with auth header
    api() {
      return axios.create({
        baseURL: "http://localhost:3000/api",
        headers: { Authorization: `Bearer ${this.token}` },
      });
    },
  },
  methods: {
    async fetchCategories() {
      try {
        const response = await axios.get("http://localhost:3000/api/categories");
        this.categories = response.data;
      } catch (error) {
        console.error("Error fetching categories:", error);
      }
    },
    async fetchComplaints() {
      try {
        const response = await axios.get("http://localhost:3000/api/complaints");
        this.complaints = response.data;
      } catch (error) {
        console.error("Error fetching complaints:", error);
      }
    },
    async fetchUsers() {
      try {
        const response2 = await axios.get("http://localhost:3000/api/users");
        this.users = response2.data;
        console.log(this.users);
        console.table(this.users);
      } catch (error) {
        console.error("Error fetching users:", error);
      }
    },
    async submitComplaint() {
      try {
        await this.api.post("/complaints", this.newComplaint);

        // Reset form
        this.newComplaint = { title: "", detail: "", category_id: "" };

        await this.fetchComplaints(); // Refresh list
      } catch (error) {
        alert("Failed to submit complaint.");
        console.error("Error submitting complaint:", error);
      }
    },
    async upvoteComplaint(complaint) {
      try {
        await this.api.put(`/complaints/upvote/${complaint.id}`);

        // Optimistically update the local score
        complaint.specificity_score++;
        // Re-sort the array manually since the score changed
        this.complaints.sort((a, b) => b.specificity_score - a.specificity_score);
      } catch (error) {
        alert("Failed to upvote complaint.");
        console.error("Error upvoting complaint:", error);
      }
    },
    async deleteComplaint(id) {
      if (!confirm("Are you sure the complaint is too generic and must be archived?"))
        return;

      try {
        await this.api.delete(`/complaints/${id}`);

        // Remove from local array
        this.complaints = this.complaints.filter((c) => c.id !== id);
      } catch (error) {
        alert(error.response.data.message || "Failed to delete complaint.");
        console.error("Error deleting complaint:", error);
      }
    },
    async banUser(id) {
      if (!confirm("Are you sure you want to ban this user ?")) return;
      try {
        await this.api.delete(`/users/delete/${id}`);

        const response = await axios.get("http://localhost:3000/api/users");
        this.users = response.data;
        const response2 = await axios.get("http://localhost:3000/api/complaints");
        this.complaints = response2.data;
      } catch (error) {
        alert(error.response.data.message || "Failed to ban user.");
        console.error("Error banning user:", error);
      }
    },
    startEdit(complaint) {
      // Create a shallow copy for editing
      this.editingComplaint = { ...complaint };
    },
    cancelEdit() {
      this.editingComplaint = null;
    },
    async updateComplaint() {
      try {
        const { id, title, detail, category_id } = this.editingComplaint;

        await this.api.put(`/complaints/${id}`, { title, detail, category_id });

        // Update the main array with the new data
        const index = this.complaints.findIndex((c) => c.id === id);
        if (index !== -1) {
          this.complaints[index].title = title;
          this.complaints[index].detail = detail;
          this.complaints[index].category_name = this.categories.find(
            (c) => c.id === category_id
          ).name;
        }
        this.cancelEdit();
      } catch (error) {
        alert("Failed to update complaint. You can only edit your own complaints.");
        console.error("Error updating complaint:", error);
      }
    },
    getImgUrl(name) {
      // Return the imported image from the map
      return this.pfpMap[name] || this.pfpMap["pfp1.jpeg"];
    },
  },
  mounted() {
    this.fetchCategories();
    this.fetchComplaints();
    this.fetchUsers();
  },
};
</script>

<style scoped>
.complaint-list {
  max-width: 800px;
  margin: 0 auto;
}
.search-and-form {
  margin-bottom: 30px;
  padding-bottom: 20px;
  border-bottom: 1px solid #eee;
}
.search-and-form input {
  padding: 10px;
  width: 100%;
  margin-bottom: 15px;
}
.new-complaint-form {
  border: 1px solid #ccc;
  padding: 15px;
  border-radius: 8px;
}
.new-complaint-form input,
.new-complaint-form textarea,
.new-complaint-form select {
  display: block;
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  box-sizing: border-box;
}
.complaint-card {
  border: 2px solid #2c3e50;
  border-radius: 6px;
  padding: 15px;
  margin-bottom: 15px;
  text-align: left;
}
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 5px;
}
.category-tag {
  background-color: #3498db;
  color: white;
  padding: 3px 8px;
  border-radius: 3px;
  font-size: 0.8em;
}
.card-footer {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  gap: 10px;
  margin-top: 10px;
  padding-top: 10px;
  border-top: 1px solid #eee;
}
.score {
  font-weight: bold;
  color: #e74c3c;
}
.delete-btn {
  background-color: #c0392b;
  color: white;
  border: none;
}
.edit-form {
  border-top: 1px solid #ddd;
  margin-top: 10px;
  padding-top: 10px;
}
.edit-form input,
.edit-form textarea,
.edit-form select {
  margin-bottom: 5px;
}
.list_user {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin: 12px 0 18px;
  padding: 12px;
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.45); /* translucent card like iOS sheets */
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border: 1px solid rgba(0, 0, 0, 0.06);
  box-shadow: 0 6px 18px rgba(15, 15, 15, 0.04);
}

/* Each user entry (direct children of .list_user) - now full-width vertical list items */
.list_user > div {
  display: flex;
  align-items: center;
  justify-content: flex-start; /* align content to the left so avatar + meta stack horizontally */
  gap: 12px;
  padding: 12px;
  width: 100%;           /* full width to form a vertical list */
  max-width: 100%;
  box-sizing: border-box;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.72);
  border: 1px solid rgba(0, 0, 0, 0.04);
  transition: transform 160ms ease, box-shadow 160ms ease;
  /* subtle separator between items */
  border-bottom: 1px solid rgba(0,0,0,0.03);
}

/* remove bottom border on last item to keep clean card look */
.list_user > div:last-child {
  border-bottom: none;
}

/* subtle hover / focus */
.list_user > div:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 30px rgba(15, 15, 15, 0.06);
}

/* avatar */
.list_user > div img {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  object-fit: cover;
  border: 1px solid rgba(0, 0, 0, 0.06);
  box-shadow: 0 4px 10px rgba(10, 10, 10, 0.05);
}

/* username + meta column */
.list_user > div p {
  margin: 0;
  font-weight: 600;
  color: #0b0b0b;
  line-height: 1;
}

/* keep action (BAN) aligned to the right */
.list_user > div button {
  margin-left: auto;
  background: transparent;
  border: none;
  color: #ff3b30;
  font-weight: 700;
  padding: 8px 10px;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 140ms ease;
}

.list_user > div button:hover,
.list_user > div button:focus {
  background: rgba(255,59,48,0.08);
  outline: none;
}

/* responsive adjustments */
@media (max-width: 680px) {
  .list_user {
    gap: 10px;
    padding: 10px;
  }
  .list_user > div {
    min-width: 100%;
    padding: 10px;
  }
}
</style>

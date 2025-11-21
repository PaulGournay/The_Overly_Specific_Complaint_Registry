<template>
  <div id="app" class="flex flex-col min-h-screen">
    <header class="app-header">
      <div class="header-left">
        <img src="./assets/logo.png" alt="Logo" class="header-logo" />
        <h1 class="header-title">The Unnecessary Complaint Registery</h1>
      </div>

      <nav class="header-nav">
        <!-- Navigation Links -->
        <div class="nav-links" v-if="!user"><!-- check if a user is logged in-->
          <button @click="currentView = 'Home'" class="nav-button">Home</button>
          <button @click="currentView = 'About'" class="nav-button">About</button>
          <button @click="currentView = 'Contact'" class="nav-button">Contact</button>
        </div>

        <!-- User Authenticated Navigation -->
        <div class="nav-links" v-else>
          <button @click="currentView = 'ComplaintList'" class="nav-button">Complains Page</button>

          <div class="user-profile-nav" @click="currentView = 'UserProfile'">
            <div class="pfp-circle">
              <img :src="getUserPfpUrl(user.pfp)" alt="PFP" class="pfp-img" />
            </div>
            <button class="user-welcome-btn">
              {{ user.username }}
            </button>
          </div>
        </div>

        <!-- Auth Buttons / Logout -->
        <div class="auth-buttons">
          <template v-if="!user">
            <button @click="currentView = 'Login'" class="btn-login">Login</button>
            <button @click="currentView = 'Register'" class="btn-register">Register</button>
          </template>
          <template v-else-if="user.role==archivist">
            <span class="user-role">(admin)</span>
          </template>
          <template v-else>
            <button @click="logout" class="btn-logout">Logout</button>
          </template>
        </div>
      </nav>
    </header>

    <div class="content flex-1">
      <HomePage v-if="currentView === 'Home'" @navigate="currentView = $event" />
      <AboutPage v-else-if="currentView === 'About'" @navigate="currentView = $event" />
      <ContactPage v-else-if="currentView === 'Contact'" @navigate="currentView = $event" />
      <PrivacyPage v-else-if="currentView === 'Privacy'" @navigate="currentView = $event" />
      <TermsPage v-else-if="currentView === 'Terms'" @navigate="currentView = $event" />
      <Register v-else-if="currentView === 'Register'" @registered="currentView = 'Login'" />
      <Login v-else-if="currentView === 'Login'" @login-success="handleLogin" />
      <ComplaintList v-else-if="user && currentView === 'ComplaintList'" :user="user" :token="token" />
      <UserProfile 
        v-else-if="user && currentView === 'UserProfile'" 
        :user="user" 
        :token="token" 
        @user-updated="handleUserUpdate" 
      />
      <div v-else-if="!user && currentView === 'ComplaintList'">
        <h2>Please Login or Register to view the complaints.</h2>
      </div>
    </div>

    <AppFooter @navigate="currentView = $event" />
  </div>
</template>

<script>
import Login from './components/UserLogin.vue';
import Register from './components/UserRegister.vue';
import ComplaintList from './components/ComplaintList.vue';
import AppFooter from './components/AppFooter.vue';
import HomePage from './components/HomePage.vue';
import AboutPage from './components/AboutPage.vue';
import ContactPage from './components/ContactPage.vue';
import PrivacyPage from './components/PrivacyPage.vue';
import TermsPage from './components/TermsPage.vue';
import UserProfile from './components/UserProfile.vue';
import pfp1 from '@/assets/pfp_image/pfp1.jpeg';
import pfp2 from '@/assets/pfp_image/pfp2.jpeg';
import pfp3 from '@/assets/pfp_image/pfp3.jpeg';
import pfp4 from '@/assets/pfp_image/pfp4.jpeg';
export default {
  name: 'App',
  components: {
    Login,
    Register,
    ComplaintList,
    AppFooter,
    HomePage,
    AboutPage,
    ContactPage,
    PrivacyPage,
    TermsPage,
    UserProfile,
  },
  data() {
    return {
      user: null, // Stores { id, username, role }
      token: null, // Stores JWT
      currentView: 'Home',
      pfpMap: {
        'pfp1.jpeg': pfp1,
        'pfp2.jpeg': pfp2,
        'pfp3.jpeg': pfp3,
        'pfp4.jpeg': pfp4
      }, 
    };
  },
  methods: {
    handleLogin(data) {
      this.user = data.user;
      this.token = data.token;
      // Store token in local storage for persistence
      localStorage.setItem('userToken', this.token);
      localStorage.setItem('userInfo', JSON.stringify(this.user));
      this.currentView = 'ComplaintList';
    },
    logout() {
      this.user = null;
      this.token = null;
      localStorage.removeItem('userToken');
      localStorage.removeItem('userInfo');
      this.currentView = 'Home';
    },
    checkLocalStorage() {
      const storedToken = localStorage.getItem('userToken');
      const storedUser = localStorage.getItem('userInfo');
      if (storedToken && storedUser) {
        this.token = storedToken;
        this.user = JSON.parse(storedUser);
        this.currentView = 'ComplaintList';
      }
    },
    handleUserUpdate(updatedUser) {
        this.user = updatedUser;
        // Update local storage so refresh keeps the new pic
        localStorage.setItem('userInfo', JSON.stringify(this.user));
    },
    getUserPfpUrl(pfpName) {
        if(!pfpName) pfpName = 'pfp1.jpeg';
        return this.pfpMap[pfpName] || this.pfpMap['pfp1.jpeg'];
    },
  },
  mounted() {
    this.checkLocalStorage();
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
}

.app-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px 30px;
  background: #ffffff;
  border-bottom: 1px solid #e0e0e0;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
}

.header-left {
  display: flex;
  align-items: center;
  gap: 15px;
  flex: 1;
}

.header-logo {
  height: 150px;
  width: 150px;
  border-radius: 8px;
  object-fit: contain;
}

.header-title {
  font-size: 30px;
  font-weight: 750;
  color: #1a1a1a;
  margin: 0;
  white-space: nowrap;
}

.header-nav {
  display: flex;
  align-items: center;
  gap: 30px;
  justify-content: flex-end;
}

.nav-links {
  display: flex;
  gap: 20px;
  align-items: center;
}

.nav-button {
  background: none;
  border: none;
  color: #555;
  font-size: 15px;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 6px;
  transition: all 0.3s ease;
  font-weight: 500;
}

.nav-button:hover {
  background: #f5f5f5;
  color: #1a1a1a;
}

.user-welcome {
  color: #1a1a1a;
  font-weight: 600;
  font-size: 15px;
}

.auth-buttons {
  display: flex;
  gap: 12px;
  align-items: center;
}

.btn-login {
  background: transparent;
  border: 2px solid #1a1a1a;
  color: #1a1a1a;
  padding: 8px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.3s ease;
}

.btn-login:hover {
  background: #1a1a1a;
  color: white;
}

.btn-register {
  background: #1a1a1a;
  border: none;
  color: white;
  padding: 8px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.3s ease;
}

.btn-register:hover {
  background: #333;
  box-shadow: 0 2px 8px rgba(26, 26, 26, 0.2);
}

.btn-logout {
  background: transparent;
  border: 2px solid #1a1a1a;
  color: #1a1a1a;
  padding: 8px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.3s ease;
}

.btn-logout:hover {
  background: #d32f2f;
  border-color: #d32f2f;
  color: white;
}

.user-role {
  color: #888;
  font-size: 13px;
}

.content {
  padding: 20px;
}
.user-profile-nav {
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  padding: 6px 12px;
  border-radius: 30px; /* Pill shape */
  border: 1px solid transparent;
  transition: all 0.2s ease;
}

.user-profile-nav:hover {
  background-color: #f8f9fa;
  border-color: #e0e0e0;
}

.pfp-circle {
  /* FORCE HEADER SIZE */
  width: 40px;
  height: 40px;
  border-radius: 50%;
  overflow: hidden; /* Cuts the image */
  border: 2px solid #e0e0e0;
  background-color: #fff;
  flex-shrink: 0; /* Prevents circle from squishing if username is long */
}

.pfp-img {
  /* Forces image to fit the 40px circle */
  width: 100%;
  height: 100%;
  object-fit: cover; /* Crops center, maintains aspect ratio */
  display: block;
}

.user-welcome-btn {
  background: none;
  border: none;
  font-weight: 600;
  font-size: 15px;
  cursor: pointer;
  color: #2c3e50;
  padding: 0;
}

/* Mobile Adjustment */

/* Responsive Design */
@media (max-width: 768px) {
  .app-header {
    flex-direction: column;
    gap: 15px;
    padding: 15px 20px;
  }

  .header-left {
    width: 100%;
    justify-content: center;
    flex-wrap: wrap;
  }

  .header-title {
    font-size: 18px;
  }

  .header-nav {
    width: 100%;
    flex-direction: column;
    gap: 15px;
  }

  .nav-links {
    width: 100%;
    justify-content: center;
    flex-wrap: wrap;
  }

  .auth-buttons {
    width: 100%;
    justify-content: center;
  }
  .user-welcome-btn {
    display: none; /* Hide name on mobile to save space, keep pfp */
  }
  
}
</style>
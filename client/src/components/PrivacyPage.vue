<template>
  <div class="ios-container">
    <div class="content-width">
      <div class="page-header">
        <h1>Privacy Policy</h1>
        <p class="header-subtitle">Your data, specific and secure.</p>
      </div>

      <div class="ios-card policy-card">
        <div class="policy-item">
          <h2>1. Information We Collect</h2>
          <p>To provide the registry service, we collect:</p>
          <ul class="ios-list">
            <li>
              <strong>Account Data:</strong> Username, Email Address, and Password
              (hashed).
            </li>
            <li><strong>Profile Data:</strong> Selected Avatar (PFP).</li>
            <li>
              <strong>Content:</strong> Complaints, Titles, Categories, and Upvotes.
            </li>
          </ul>
        </div>
        <div class="separator"></div>

        <div class="policy-item">
          <h2>2. How We Use Your Data</h2>
          <p>
            We use your data to maintain your profile, calculate Specificity Scores, and
            allow the Archivist to manage the community. We do not sell your data.
          </p>
        </div>
        <div class="separator"></div>

        <div class="policy-item">
          <h2>3. Data Security</h2>
          <p>
            We take security seriously. Passwords are hashed using
            <strong>Bcrypt</strong>. Authentication is handled via
            <strong>JSON Web Tokens (JWT)</strong>. All data is stored in a secure SQL
            database.
          </p>
        </div>
        <div class="separator"></div>

        <div class="policy-item">
          <h2>4. Your Rights</h2>
          <p>
            You have the right to access and update your profile information (Username,
            Email, PFP) at any time via the User Profile page. You may also request
            account deletion by contacting an Archivist.
          </p>
        </div>
        <div class="separator"></div>

        <div class="policy-item">
          <h2>5. Cookies & Local Storage</h2>
          <p>
            We use Local Storage to keep you logged in. We do not use third-party tracking
            cookies.
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* --- Global iOS Layout --- */
.ios-container {
  background-color: #f2f2f7;
  min-height: 100vh;
  padding: 40px 20px 80px;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial,
    sans-serif;
}

.content-width {
  max-width: 800px;
  margin: 0 auto;
}

.page-header {
  text-align: center;
  margin-bottom: 40px;
}

.page-header h1 {
  font-size: 36px;
  font-weight: 800;
  color: #1c1c1e;
  margin: 0 0 5px 0;
}

.header-subtitle {
  font-size: 16px;
  color: #8e8e93;
}

.ios-card {
  background: white;
  border-radius: 20px;
  padding: 0 24px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.03);
}

.policy-item {
  padding: 24px 0;
}

.policy-item h2 {
  font-size: 17px;
  font-weight: 600;
  color: #1c1c1e;
  margin: 0 0 8px 0;
}

.policy-item p {
  font-size: 15px;
  color: #3a3a3c;
  line-height: 1.5;
  margin: 0;
}

.ios-list {
  margin: 8px 0 0 0;
  padding-left: 20px;
  color: #3a3a3c;
  font-size: 15px;
}
.ios-list li {
  margin-bottom: 4px;
}

.separator {
  height: 1px;
  background-color: #e5e5ea;
}
</style>

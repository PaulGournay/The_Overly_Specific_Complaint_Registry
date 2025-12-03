<template>
  <div class="ios-container">
    <div class="content-width">
      <div class="page-header">
        <h1>Terms of Service</h1>
        <p class="header-subtitle">Last Updated: Dec 2025</p>
      </div>

      <div class="ios-card policy-card">
        <div class="policy-item">
          <h2>1. Acceptance of Terms</h2>
          <p>
            By accessing The Unnecessary Complaint Registry, you agree to these terms.
            This project is for educational purposes.
          </p>
        </div>
        <div class="separator"></div>

        <div class="policy-item">
          <h2>2. User Accounts</h2>
          <p>
            You are responsible for your account security. We now require a valid email
            address for account recovery purposes.
          </p>
        </div>
        <div class="separator"></div>

        <div class="policy-item">
          <h2>3. User Conduct</h2>
          <p>Users must not:</p>
          <ul class="ios-list">
            <li>Post illegal or abusive content.</li>
            <li>Spam the registry with non-specific complaints.</li>
            <li>Attempt to hack the specificity scoring system.</li>
          </ul>
        </div>
        <div class="separator"></div>

        <div class="policy-item">
          <h2>4. Content Ownership</h2>
          <p>
            You own your complaints, but you grant us the right to display them publicly
            and allow others to vote on them.
          </p>
        </div>
        <div class="separator"></div>

        <div class="policy-item">
          <h2>5. Archivist (Admin) Powers</h2>
          <p>Archivists act as moderators. They have the right to:</p>
          <ul class="ios-list">
            <li>Delete any complaint deemed "too generic."</li>
            <li>Ban users who violate community standards.</li>
            <li>Reset vote counts on suspicious entries.</li>
          </ul>
        </div>
        <div class="separator"></div>

        <div class="policy-item">
          <h2>6. Limitation of Liability</h2>
          <p>
            The registry is provided "as is." We are not liable for emotional distress
            caused by reading about wet socks.
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
  padding: 0 24px; /* Padding for horizontal, vertical handled by items */
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

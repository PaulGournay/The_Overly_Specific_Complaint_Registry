<template>
  <div class="ios-container">
    <div class="content-width">
      <div class="page-header">
        <h1>About Us</h1>
        <p class="header-subtitle">The team behind the grievances.</p>
      </div>

      <section class="ios-section">
        <h2 class="section-label">The Creators</h2>
        <div class="ios-card team-card">
          <p class="intro-text">
            We are <strong>EFREI Paris</strong> students currently on a mobility study
            exchange at <strong>APU Kuala Lumpur</strong>. This project was developed for
            our Advanced Web Programming course.
          </p>

          <div class="team-grid">
            <div class="team-member">
              <div class="avatar-wrapper">
                <img src="../assets/about_image/about_Paul.png" alt="Paul" />
              </div>
              <span class="member-name">Paul GOURNAY</span>
            </div>

            <div class="team-member">
              <div class="avatar-wrapper">
                <img src="../assets/about_image/about_Maxime.jpeg" alt="Maxime" />
              </div>
              <span class="member-name">Maxime BERGER</span>
            </div>

            <div class="team-member">
              <div class="avatar-wrapper">
                <img src="../assets/about_image/about_Clayton.png" alt="Clayton" />
              </div>
              <span class="member-name">Clayton <br />HABYALIMANA</span>
            </div>
          </div>
        </div>
      </section>

      <section class="ios-section">
        <h2 class="section-label">Our Philosophy</h2>
        <div class="ios-card info-card">
          <div class="info-block">
            <h3>Our Mission</h3>
            <p>
              To provide a safe haven for the expression of extraordinarily specific
              complaints. We believe that every grievance, no matter how niche, deserves
              to be documented, shared, and scored.
            </p>
          </div>
          <div class="separator"></div>
          <div class="info-block">
            <h3>Why This Exists</h3>
            <p>
              That specific sound your refrigerator makes. The way shoelaces untie
              themselves. These aren't just annoyances; they are shared human experiences.
              We catalogue them.
            </p>
          </div>
        </div>
      </section>

      <section class="ios-section">
        <h2 class="section-label">The Ecosystem</h2>
        <div class="ios-card info-card">
          <div class="info-row">
            <div class="icon-box blue">👥</div>
            <div>
              <h3>Complainers</h3>
              <p>Users who lodge complaints and upvote entries based on relatability.</p>
            </div>
          </div>
          <div class="separator"></div>
          <div class="info-row">
            <div class="icon-box purple">🛡️</div>
            <div>
              <h3>Archivists (Admins)</h3>
              <p>Guardians of the registry who maintain order and banish spam.</p>
            </div>
          </div>
          <div class="separator"></div>
          <div class="info-row">
            <div class="icon-box red">💯</div>
            <div>
              <h3>Specificity Score</h3>
              <p>
                Our unique metric. The more specific and relatable a complaint, the higher
                it climbs.
              </p>
            </div>
          </div>
        </div>
      </section>
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

/* --- Headers --- */
.page-header {
  text-align: center;
  margin-bottom: 40px;
}

.page-header h1 {
  font-size: 40px;
  font-weight: 800;
  color: #1c1c1e;
  margin: 0 0 5px 0;
  letter-spacing: -1px;
}

.header-subtitle {
  font-size: 18px;
  color: #8e8e93;
  margin: 0;
}

.section-label {
  font-size: 13px;
  color: #6e6e73;
  text-transform: uppercase;
  margin-bottom: 8px;
  margin-left: 15px;
  letter-spacing: 0.5px;
}

/* --- Cards --- */
.ios-card {
  background: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.03);
  margin-bottom: 30px;
}

.team-card {
  padding: 30px;
}
.info-card {
  padding: 0 20px;
} /* Padding handled by children */

/* --- Team Styling --- */
.intro-text {
  text-align: center;
  color: #3a3a3c;
  line-height: 1.5;
  margin-bottom: 30px;
}

.team-grid {
  display: flex;
  justify-content: center;
  gap: 30px;
  flex-wrap: wrap;
}

.team-member {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.avatar-wrapper {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  overflow: hidden;
  border: 3px solid #f2f2f7;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.avatar-wrapper img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.member-name {
  font-weight: 600;
  font-size: 15px;
  color: #1c1c1e;
  text-align: center;
}

/* --- Info Blocks --- */
.info-block {
  padding: 20px 0;
}

.info-block h3 {
  margin: 0 0 5px 0;
  font-size: 17px;
  color: #1c1c1e;
}

.info-block p {
  margin: 0;
  color: #8e8e93;
  line-height: 1.4;
  font-size: 15px;
}

.info-row {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 15px 0;
}

.info-row h3 {
  font-size: 16px;
  margin: 0;
  color: #1c1c1e;
}

.info-row p {
  font-size: 13px;
  margin: 2px 0 0 0;
  color: #8e8e93;
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
.icon-box.purple {
  background-color: #f3e5f5;
}
.icon-box.red {
  background-color: #fff0f2;
}

.separator {
  height: 1px;
  background-color: #e5e5ea;
}
</style>

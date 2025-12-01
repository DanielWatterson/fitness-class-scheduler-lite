<template>

  <!-- FORMATTED WITH PRETTIER -->
  <div class="app">
    <!-- Header -->
    <header class="header">
      <div class="nav-bar">
        <div class="logo">💪 FlexZone Ltd.</div>
        <h1 class="title">Fitness Scheduler</h1>
        <div class="filters">
          <button @click="showAll = true" :class="{ active: showAll }">All Sessions</button>
          <button @click="showAll = false" :class="{ active: !showAll }">Upcoming Only</button>
        </div>
      </div>

      <!-- Search bar -->
      <div class="search-bar">
        <input v-model="coachFilter" placeholder="Filter by coach name" />
      </div>

      <!-- Total class count and next session -->
      <div class="session-info">
        <p>
          Total Classes: <strong>{{ sessions.length }}</strong>
        </p>
      </div>
    </header>

    <!-- MAIN LAYOUT (2 COLUMNS) -->
    <div class="layout">
      <!-- LEFT SIDE – Add Session -->
      <div class="left-panel">
        <AddSessionForm @add-session="addSession" />
      </div>

      <!-- RIGHT SIDE – Sessions -->
      <div class="right-panel">
        <!-- Next upcoming session box -->
        <div v-if="nextSession" class="next-session-box">
          <p>Next Session:</p>
          <p>
            <strong>{{ nextSession.name }}</strong> with <strong>{{ nextSession.coach }}</strong> on
            {{ nextSession.date }} @ {{ nextSession.time }}
          </p>
        </div>

        <!-- Empty state -->
        <div v-if="filteredSessions.length === 0">
          <EmptyState />
        </div>

        <!-- Sessions grid with animation -->
        <transition-group name="fade" tag="div" class="sessions-grid" v-else>
          <SessionCard
            v-for="session in filteredSessions"
            :key="session.id"
            :session="session"
            @delete-session="deleteSession"
          />
        </transition-group>
      </div>
    </div>
    <footer class="footer">© 2025 • FlexZone Ltd. • All Rights Reserved</footer>
  </div>
</template>

<script>
import AddSessionForm from './components/AddSessionForm.vue'
import SessionCard from './components/SessionCard.vue'
import EmptyState from './components/EmptyState.vue'

export default {
  name: 'App',
  components: { AddSessionForm, SessionCard, EmptyState },

  /**
   * App component data
   * @property {Array} sessions - an array of scheduled classes
   * @property {Boolean} showAll - a flag to show all classes or only upcoming ones
   * @property {String} coachFilter - a filter text to search for classes by coach name
   */
  data() {
    return {
      sessions: [],
      showAll: true,
      coachFilter: '',
    }
  },

  created() {
    const saved = localStorage.getItem('sessions')
    if (saved) this.sessions = JSON.parse(saved)
  },

  computed: {
    filteredSessions() {
      const now = new Date()
      let filtered = this.showAll
        ? this.sessions
        : this.sessions.filter((s) => new Date(s.date + ' ' + s.time) >= now)

      if (this.coachFilter) {
        filtered = filtered.filter((s) =>
          s.coach.toLowerCase().includes(this.coachFilter.toLowerCase()),
        )
      }
      return filtered
    },

    nextSession() {
      const now = new Date()
      const upcoming = this.sessions
        .filter((s) => new Date(s.date + ' ' + s.time) >= now)
        .sort((a, b) => new Date(a.date + ' ' + a.time) - new Date(b.date + ' ' + b.time))
      return upcoming[0] || null
    },
  },

  methods: {
    /**
     * Add a new session to the list
     * @param {Object} session - an object containing the session details
     * @example
     * addSession({
     *   id: 1,
     *   name: 'Yoga',
     *   coach: 'John Doe',
     *   date: '2022-01-01',
     *   time: '10:00',
     *   capacity: 20,
     * })
     */
    addSession(session) {
      this.sessions.push(session)
      localStorage.setItem('sessions', JSON.stringify(this.sessions))
    },
    deleteSession(id) {
      this.sessions = this.sessions.filter((s) => s.id !== id)
      localStorage.setItem('sessions', JSON.stringify(this.sessions))
    },
  },
}
</script>

<style>
/* This is for a Full-page reset */
html,
body {
  margin: 0;
  padding: 0;
  min-height: 100%;
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(to bottom, #fffaf0, #ffe6e1);
}

/* ----------------- Main wrapper ----------------- */
.app {
  max-width: 1600px;
  margin: auto;
  padding: 10px 20px;
}

/* ----------------- HEADER ----------------- */
.header {
  background: linear-gradient(90deg, #ff7e5f, #feb47b);
  padding: 18px 25px;
  border-radius: 12px;
  color: white;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  margin-bottom: 20px;
}

.nav-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}

.logo {
  font-size: 1.7rem;
  font-weight: 700;
}

.title {
  font-size: 1.5rem;
  flex: 1;
  text-align: center;
  font-weight: 600;
}

.filters button {
  padding: 8px 18px;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-weight: 600;
  background: white;
  color: #ff6347;
  margin-left: 10px;
  transition: 0.25s ease;
}

.filters button.active {
  background: #ff4500;
  color: white;
}

.filters button:hover {
  transform: translateY(-2px);
}

/* ----------------- SEARCH BAR ----------------- */
.search-bar {
  margin-top: 10px;
}

.search-bar input {
  padding: 8px 12px;
  width: 250px;
  border-radius: 8px;
  border: 1px solid #ccc;
  font-weight: 500;
}

/* ----------------- SESSION INFO ----------------- */
.session-info {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  margin-top: 12px;
  font-weight: 600;
  color: #ff4500;
}

/* ----------------- LAYOUT ----------------- */
.layout {
  display: grid;
  grid-template-columns: 280px 1fr;
  gap: 20px;
  margin-bottom: 40px;
}

/* ----------------- Left panel (smaller) ----------------- */
.left-panel {
  background: white;
  padding: 16px;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  height: fit-content;
  min-height: 300px;
}

/* ----------------- Right panel – white box for sessions ----------------- */
.right-panel {
  background: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  min-height: 300px;
}

/* ----------------- Next session box ----------------- */
.next-session-box {
  background: #fffaf0;
  padding: 12px 16px;
  border-radius: 10px;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.08);
  margin-bottom: 18px;
  font-weight: 600;
  color: #ff4500;
}

/* ----------------- Session cards grid ----------------- */
.sessions-grid {
  display: grid;
  gap: 18px;
  grid-template-columns: 1fr 1fr;
}

/* ----------------- DIFFERENT DEVICES ----------------- */

/* Tablets */
@media (max-width: 1100px) {
  .layout {
    grid-template-columns: 1fr;
  }
  .sessions-grid {
    grid-template-columns: 1fr;
  }
  .left-panel {
    max-width: 500px;
    margin: auto;
    width: 100%;
  }
}

/* Phones */
@media (max-width: 700px) {
  .title {
    font-size: 1.2rem;
  }
  .logo {
    font-size: 1.4rem;
  }
}

/* ----------------- FOOTER ----------------- */
.footer {
  text-align: center;
  padding: 15px 0;
  margin-top: 40px;
  color: #444;
  font-size: 0.9rem;
  opacity: 0.7;
}

/* ----------------- TRANSITIONS ----------------- */
.fade-enter-active,
.fade-leave-active {
  transition: all 0.4s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>

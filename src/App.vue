<template>
  <div>
    <h1>FlexZone Fitness Scheduler</h1>

    <AddSessionForm @add-session="addSession" />

    <div v-if="sessions.length === 0">
      <EmptyState />
    </div>

    <SessionList
      v-else
      :sessions="sessions"
      @delete-session="deleteSession"
    />
  </div>
</template>

<script>
import AddSessionForm from "./components/AddSessionForm.vue";
import SessionList from "./components/SessionList.vue";
import EmptyState from "./components/EmptyState.vue";

export default {
  components: { AddSessionForm, SessionList, EmptyState },

  data() {
    return {
      sessions: []
    };
  },

  created() {
    const saved = localStorage.getItem("sessions");
    if (saved) {
      this.sessions = JSON.parse(saved);
    }
  },

  methods: {
    addSession(session) {
      this.sessions.push(session);
      localStorage.setItem("sessions", JSON.stringify(this.sessions));
    },

    deleteSession(id) {
      this.sessions = this.sessions.filter((s) => s.id !== id);
      localStorage.setItem("sessions", JSON.stringify(this.sessions));
    }
  }
};
</script>

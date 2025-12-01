<template>
  <div class="session-list">
    <SessionCard
      v-for="session in sessions"
      :key="session.id"
      :session="session"
      @delete-session="$emit('delete-session', $event)"
    />
  </div>
</template>

<script>
import SessionCard from "./SessionCard.vue";

export default {
  name: "SessionList",
  components: { SessionCard },
  props: ["sessions"]
};
</script>

<style scoped>
.session-list {
  display: grid;
  gap: 20px;
}
</style>

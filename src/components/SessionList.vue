<template>
  <div>
    <SessionCard
      v-for="session in sessions"
      :key="session.id"
      :session="session"
      @delete-session="$emit('delete-session', session.id)"
    />
  </div>
</template>

<script>
import SessionCard from "./SessionCard.vue";

export default {
  props: ["sessions"],
  components: { SessionCard }
};
</script>
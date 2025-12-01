<template>
  <div>
    <p>{{ session.name }}</p>
    <p>{{ session.coach }}</p>
    <p>{{ session.date }}</p>
    <p>{{ session.time }}</p>
    <p>{{ session.capacity }}</p>

    <button @click="$emit('delete-session', session.id)">
      Delete
    </button>
  </div>
</template>

<script>
export default {
  props: ["session"]
};
</script>

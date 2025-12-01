<template>
  <div class="session-card">
    <h2 class="name">{{ session.name }}</h2>
    <p class="coach">Coach: {{ session.coach }}</p>
    <p class="datetime">{{ session.date }} @ {{ session.time }}</p>
    <p class="capacity">Capacity: {{ session.capacity }}</p>

    <div class="button-row">
      <button class="delete-btn" @click="$emit('delete-session', session.id)">
        Delete
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "SessionCard",
  props: ["session"]
};
</script>

<style scoped>
.session-card {
  background: #fff;
  border-radius: 10px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  box-shadow: 0 4px 14px rgba(0,0,0,0.1);
  transition: transform 0.15s ease;
}

.session-card:hover {
  transform: translateY(-3px);
}

.name {
  font-weight: 700;
  font-size: 1.1rem;
  margin-bottom: 5px;
}

.coach, .datetime, .capacity {
  font-size: 0.95rem;
  margin-bottom: 4px;
  color: #555;
}

.button-row {
  margin-top: auto;
  display: flex;
  gap: 10px;
}

.delete-btn {
  flex: 1;
  padding: 10px 0;
  border-radius: 6px;
  border: none;
  font-weight: 700;
  cursor: pointer;
  transition: 0.15s ease;
  background: #ff6347;
  color: white;
}

.delete-btn:hover {
  background: #ff4b2b;
}
</style>

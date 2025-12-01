<template>
  <div class="empty-state">
    <p>📅 No sessions scheduled yet.</p>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.empty-state {
  min-height: calc(100vh - 200px); /* Adjust 200px if header/form height differs */
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(to bottom, #fffaf0, #ffe6e1);
  border-radius: 12px;
  padding: 40px;
  text-align: center;
  font-size: 1.3rem;
  color: #ff4500;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
  transition: transform 0.15s ease;
}

.empty-state:hover {
  transform: translateY(-2px);
}
</style>

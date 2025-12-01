<template>
  <form class="add-form" @submit.prevent="handleSubmit">
    <input v-model="name" placeholder="Class Name" />
    <input v-model="coach" placeholder="Coach Name" />
    <input type="date" v-model="date" />
    <input type="time" v-model="time" />
    <input v-model="capacity" placeholder="Capacity" />

    <button type="submit">Add Session</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      coach: "",
      date: "",
      time: "",
      capacity: ""
    };
  },
  methods: {
    handleSubmit() {
      if (!this.name || !this.coach || !this.date || !this.time || !this.capacity) return;

      const newSession = {
        id: Date.now(),
        name: this.name,
        coach: this.coach,
        date: this.date,
        time: this.time,
        capacity: this.capacity
      };

      this.$emit("add-session", newSession);

      this.name = "";
      this.coach = "";
      this.date = "";
      this.time = "";
      this.capacity = "";
    }
  }
};
</script>

<style scoped>
.add-form {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 25px;
}

.add-form input {
  flex: 1 1 150px;
  padding: 8px 12px;
  border-radius: 8px;
  border: 1px solid #ccc;
}

.add-form button {
  padding: 10px 20px;
  border-radius: 8px;
  border: none;
  background: #ff6347;
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: 0.2s ease;
}

.add-form button:hover {
  background: #ff4b2b;
}
</style>

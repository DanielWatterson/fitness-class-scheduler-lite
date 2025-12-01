<template>
  <form @submit.prevent="handleSubmit">
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
      if (!this.name || !this.coach || !this.date || !this.time || !this.capacity) {
        return;
      }

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

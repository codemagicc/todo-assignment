<template>
  <div id="task-form">
    <form @submit.prevent="handleSubmit">
      <label>Title</label>
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && isNameValid }"
        v-model="task.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Description</label>
      <input
        type="text"
        :class="{ 'has-error': submitting && isEmailValid }"
        v-model="task.email"
        @focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">
        ❗Please fill out all required fields and valid email address
      </p>
      <p v-if="success" class="success-message">
        ✅ Task successfully added
      </p>
      <button>Add Task</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "TodoForm",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      task: {
        name: "",
        email: "",
        completed: false,
      },
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.isNameValid || this.isEmailValid) {
        this.error = true;
        return;
      }

      this.$emit("addTask", this.task);
      this.$refs.first.focus();
      this.task = {
        name: "",
        email: "",
      };
      this.error = false;
      this.success = true;
      setTimeout(() => {
        this.success = false;
      }, 3000);
      this.submitting = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    },
  },

  computed: {
    isNameValid() {
      return this.task.name === "";
    },
    isEmailValid() {
      return this.task.email === "";
    },
  },
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>

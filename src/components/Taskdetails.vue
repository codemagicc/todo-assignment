<template>
  <div id="tasks-details">
    <p style="text-align: center; font-size: 20px; color: green;" v-if="saved">
      Details Saved successfully
    </p>

    <p v-if="tasks.length < 1">
      No Task Added
    </p>

    <table v-else>
      <thead>
        <tr>
          <th>Title</th>
          <th>Description</th>
          <th style="padding-left: 53px;">Actions</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td v-if="editing === task.id">
            <input type="text" v-model="task.name" />
          </td>
          <td v-else>
            <input type="checkbox" v-model="task.completed" />
            {{ task.name }}
          </td>

          <td v-if="editing === task.id">
            <input type="text" v-model="task.email" />
          </td>
          <td v-else>
            {{ task.email }}
          </td>
          <td v-if="editing === task.id">
            <button @click="editTask(task)">Save</button>
            <button @click="cancelEdit(task)" class="muted-button">
              Cancel
            </button>
          </td>

          <td v-else class="cell-width">
            <button @click="editMode(task)">Edit</button>
            <button
              @click="$emit('delete:task', task.id)"
              class="delete-button"
            >
              Delete
            </button>
          </td>

          <td v-if="task.completed" class="stat-complete">
            {{ task.completed ? "completed" : "incomplete" }}
          </td>
          <td v-else class="stat-incomplete">incomplete</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "task-details",
  props: {
    tasks: Array,
  },

  data() {
    return {
      editing: null,
      saved: false,
      clicked: true,
    };
  },

  methods: {
    editMode(task) {
      this.cachedEmployee = Object.assign({}, task);
      this.editing = task.id;
      if (task.completed) {
        this.toggleCheck(task);
      }
      this.clicked = false;
    },

    cancelEdit(task) {
      Object.assign(task, this.cachedEmployee);
      this.editing = null;
      if (task.completed) {
        this.toggleCheck(task);
      }
    },

    editTask(task) {
      if (task.name === "" || task.email === "") return;
      this.$emit("edit:task", task);
      this.editing = null;
      this.saved = true;
      setTimeout(() => {
        this.saved = false;
      }, 2000);
    },
    toggleCheck(task) {
      task.completed = !task.completed;
    },
  },
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}

input {
  margin: 0;
}

.empty-table {
  text-align: center;
}

.stat-complete {
  color: green;
}

.stat-incomplete {
  color: red;
}
</style>

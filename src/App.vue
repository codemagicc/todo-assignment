<template>
  <div id="app" class="small-container">
    <div class="container">
      <h1 style="text-align: center;">Task Details</h1>
      <TodoForm @addTask="addTask" />

      <TaskDetails
        v-bind:tasks="todo_list"
        @edit:task="editToDoTask"
        @delete:task="deleteToDoTask"
      />
    </div>
  </div>
</template>

<script>
import TodoForm from "./components/Todoform.vue";
import TaskDetails from "./components/Taskdetails.vue";

export default {
  name: "app",
  components: {
    TaskDetails,
    TodoForm,
  },
  data() {
    return {
      todo_list: [],
    };
  },
  methods: {
    addTask(task) {
      const newId = this.todo_list.length + 1;
      const newTask = { id: newId, ...task };
      this.todo_list.unshift(newTask);
      localStorage.setItem("todo_list", JSON.stringify(this.todo_list));
    },

    editToDoTask(task) {
      const index = this.todo_list.findIndex((item) => item.id === task.id);
      if (index !== -1) {
        this.todo_list[index] = {
          ...task,
        };
        localStorage.setItem("todo_list", JSON.stringify(this.todo_list));
      }
    },

    deleteToDoTask(id) {
      this.todo_list = this.todo_list.filter((task) => task.id !== id);
      localStorage.setItem("task", JSON.stringify(this.todo_list));
    },
  },
  created() {
    const localData = localStorage.getItem("todo_list");
    if (localData) {
      this.todo_list = JSON.parse(localData);
    }
  },
};
</script>

<style>
.small-container {
  max-width: 911px !important;

  margin-left: 12rem !important;
}
button {
  background: #41b883;
  border: 1px solid #41b883;
}

.delete-button {
  background: #d11a2a;
  border: 1px solid #d11a2a;
}

.small-container {
  max-width: 720px;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 10px;
  margin-left: 10px;
}
</style>

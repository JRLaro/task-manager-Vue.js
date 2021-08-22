<template>
  <div>
    <TaskAvail :tasks="tasks" />
    <AddTaskForm v-show="showAddTask" @add-task="addTask" />
    <Tasks
      @toggle-reminder="toogleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>


<script>
import Tasks from "../components/Tasks.vue";
import AddTaskForm from "../components/AddTaskForm.vue";
import TaskAvail from "../components/TaskAvail.vue";
import axios from "axios";

export default {
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTaskForm,
    TaskAvail,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async addTask(task) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: { "Content-type": "application/json" },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      this.tasks = [data, ...this.tasks];
    },

    async deleteTask(id) {
      try {
        if (confirm("Are you sure?")) {
          const res = await fetch(`api/tasks/${id}`, {
            method: "DELETE",
          });

          res.status === 200
            ? (this.tasks = this.tasks.filter((task) => task.id !== id))
            : alert("Error deleting task");
        }
      } catch (err) {
        console.log(err.res.data);
      }
    },

    async toogleReminder(id) {
      const taskToToogle = await this.fetchTask(id);
      const updTask = { ...taskToToogle, reminder: !taskToToogle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });
      const data = await res.json();

      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    async fetchTasks() {
      try {
        const res = await axios.get("api/tasks");
        return res.data;
      } catch (err) {
        console.error(err.res.data);
      }
    },
    async fetchTask(id) {
      try {
        const res = await axios.get(`api/tasks/${id}`);
        return res.data;
      } catch (err) {
        console.log(err.res.data);
      }
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
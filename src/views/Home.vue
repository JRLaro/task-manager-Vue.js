<template>
  <div>
    <TaskAvail v-show="tasks.length > 0" :tasks="tasks" />
    <AddTaskForm v-show="showAddTask" @add-task="addTask" />

    <div>
      <div v-if="isLoading" class="loading">
        <Loading />
      </div>

      <p
        class="isLoadingP"
        v-else-if="!isLoading && (!tasks || tasks.length === 0)"
      >
        You have No Task at this time.
      </p>

      <Tasks
        v-if="!isLoading"
        @toggle-reminder="toogleReminder"
        @delete-task="deleteTask"
        @update-task="updateTask"
        :tasks="tasks"
      />
    </div>
  </div>
</template>


<script>
import Tasks from "../components/Tasks.vue";
import AddTaskForm from "../components/AddTaskForm.vue";
import TaskAvail from "../components/TaskAvail.vue";
import Loading from "../components/Loading";
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
    Loading,
  },
  data() {
    return {
      tasks: [],
      isLoading: false,
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
      this.tasks = [...this.tasks, data];
    },
    // async updateTask(id) {
    //   try {
    //     const res = await fetch(`api/tasks/${id}`, {
    //       method: "PUT",
    //       headers: {
    //         "Content-type": "application/json",
    //       },
    //       body: JSON.stringify(task),
    //     });
    //     const data = await res.json();

    //   this.tasks = this.tasks.map((task) =>
    //     task.id === id ? { ...task, data } : task
    //   );
    //   } catch (err) {

    //   }
    // },
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
      const updReminder = { ...taskToToogle, reminder: !taskToToogle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updReminder),
      });
      const data = await res.json();

      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    async fetchTasks() {
      this.isLoading = true;
      try {
        const res = await axios.get("api/tasks");
        this.isLoading = false;
        return res.data;
        // this.tasks = [...this.tasks, res.data];
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

<style>
.isLoadingP {
  display: flex;
  justify-content: center;
}

.loading {
  display: flex;
  justify-content: center;
}
</style>
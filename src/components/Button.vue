<template>
  <div>
    <button @click="taskLog" :style="{ backgroundColor: color }" class="btn">
      <i :class="!showAddTask ? 'fas fa-plus' : 'fas fa-times-circle'"></i>
      {{ text }}
    </button>
  </div>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: String,
    color: String,
    showAddTask: Boolean,
  },
  methods: {
    taskLog() {
      this.$emit("btn-click");
    },
  },
};
</script>

<style></style>

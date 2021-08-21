<template>
  <div>
    <p>Tasks available: {{ tasksAmount }}</p>
    <span v-show="showCongrats"> 5 or more tasks created.</span>
  </div>
</template>

<script>
export default {
  name: "TaskAvail",
  props: {
    tasks: Array,
  },
  data() {
    return {
      showCongrats: false,
    };
  },
  computed: {
    tasksAmount() {
      return this.tasks.length;
    },
  },
  watch: {
    tasksAmount(val) {
      this.showCongrats = val >= 5;
    },
  },
};

/**
 * <span v-if="showCongratulations">conasdfadf</span>
 * <div style="
 
"><span>Tasks available: 3</span>
<span></span></div>
 */
</script>

<style scoped>
div {
  display: flex;
  justify-content: space-between;
}
</style>

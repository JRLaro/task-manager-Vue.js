<template>
  <div
    @dblclick="$emit('toggle-reminder', task.id)"
    :class="[task.reminder ? 'reminder' : '', 'task']"
  >
    <span
      >
      <!-- <img src="https://randomuser.me/api/portraits/thumb/men/75.jpg" alt="random picture"> -->
      <img :src="task.img" >
</span>
    <h3>
      {{ task.text }}
      <i @click="$emit('delete-task', task.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>

<script>


export default {
  name: "Task",
  props: {
    task: Object,
  },
  methods: {
    thisImg() {
      console.log('====================================')
      console.log(this.task.img);
      console.log('====================================')
    }
  },
  watch:{
    task(val){
      console.log(val.img)
    }
  }
};
</script>

<style scope>
img {
  border-radius: 50%;
}
.fa-times {
  color: red;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}

.task.reminder:hover {
  border-left: 25px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>

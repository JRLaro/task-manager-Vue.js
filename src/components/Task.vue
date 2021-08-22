<template>
  <div
    @dblclick="$emit('toggle-reminder', task.id)"
    :class="[task.reminder ? 'reminder' : '', 'task']"
  >
    <span
      >
      <img :src="task.img" >
      <span>
      <i @click="$emit('delete-task', task.id)" class="fas fa-edit"></i>
      <i @click="$emit('delete-task', task.id)" class="fas fa-times"></i>
      </span>
</span>
    <h3>
      {{ task.text }}
    </h3>
    <p>{{ task.day }}</p>
    <h3>
      {{ task.createdBy}}
    </h3>
  </div>
</template>

<script>


export default {
  name: "Task",
  props: {
    task: Object,
  },
};
</script>

<style scoped>
img {
  border-radius: 50%;
}

i{
  margin-left:10px;
}

span {
display: flex;
justify-content: space-between;
}

.fa-times {
  color: red;
}

.fa-edit {
  color: rgb(19, 155, 28);
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

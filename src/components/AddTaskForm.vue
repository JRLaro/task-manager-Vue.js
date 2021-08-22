<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input v-model="text" type="text" name="text" placeholder="Add Task" />
    </div>
    <div class="form-control">
      <label>Day & Time</label>
      <input
        v-model="day"
        type="text"
        name="day"
        placeholder="Add Day & Time"
      />
    </div>
    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input v-model="reminder" type="checkbox" name="reminder" />
    </div>

    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script>
import axios from "axios";

export default {
  name: "AddTaskForm",
  data() {
    return {
      img: "",
      text: "",
      day: "",
      reminder: false,
    };
  },
  methods: {
    async onSubmit(e) {
      e.preventDefault();
      // console.log(`Form submitted`);

      if (!this.text) {
        alert("Please add a Task");
        return;
      }

      const res = await axios.get("https://randomuser.me/api/");
      const thisImg = res.data.results[0].picture.thumbnail;

      const newTask = {
        // img: thisImg,
        img: thisImg,
        text: this.text,
        day: this.day,
        reminder: this.reminder,
      };
      // console.log('///------------->')
      //   console.log(thisImg)
      // console.log('///------------->')

      this.$emit("add-task", newTask);
      this.img = "";
      this.text = "";
      this.day = "";
      this.reminder = false;

      // return console.log(this.img);
    },
    // async getImg() {
    //   const res = await axios.get("https://randomuser.me/api/");
    //   const thisImg = res.data.results[0].picture.thumbnail;
    //   this.img = thisImg;

    //   // return thisImg;
    //   return this.img;
    // },
  },
  randoNum() {
    Math.floor(Math.random() * 10000);
  },
};
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>

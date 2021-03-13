<template>
  <div>
    <h1>{{message}}</h1>
    <form @submit.prevent="postStudent">
      <input type="text" v-model="nowInputedSutdentName" />
      <button>add a student name</button>
    </form>
    <ul>
      <li v-for="(stu, index) of  students" :key="index">
        {{stu}}
        <button @click="delStudent(index)">delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      message: "hello vue",
      students: [],
      nowInputedSutdentName: ""
    };
  },
  created() {
    //test cors
    // axios.get("http://127.0.0.1:7001/test").then(
    //   res => {
    //     console.log(res);
    //     this.message = res.data;
    //   },
    //   err => {
    //     console.log(err);
    //   }
    // );

    //get students
    this.getStudent();
  },
  methods: {
    getStudent() {
      axios.get("http://127.0.0.1:7001/students").then(
        res => {
          console.log(res);
          this.students = res.data;
        },
        err => {
          console.log(err);
        }
      );
    },
    postStudent() {
      axios
        .post("http://127.0.0.1:7001/students", {
          studentName: this.nowInputedSutdentName
        })
        .then(() => {
          //reset  this.nowInputedSutdentName
          this.nowInputedSutdentName = "";
          this.getStudent();
        });
    },
    //need checked later
    delStudent(index) {
      axios
        .delete(`http://127.0.0.1:7001/students/${index}`)
        .then(this.getStudent);
    }
  }
};
</script>

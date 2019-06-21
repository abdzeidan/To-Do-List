<template>
  <div>
    <h1>To Do List</h1>
    <TodoForm @newTodo="addTodo">
      <h2 slot="title">Add a To-Do</h2>
      <p slot="desc">Your To-Dos will be saved.</p>
    </TodoForm>
    <Todo :todos="todoList" @removeTodo="appDeleteTodo"/>
  </div>
</template>
<script>
import Todo from "@/components/Todo.vue";
import TodoForm from "@/components/TodoForm.vue";
import axios from "axios";

export default {
  data() {
    return {
      todoList: ["Walk the Dog", "Go for a Ride"]
    };
  },
  components: {
    Todo,
    TodoForm
  },
  methods: {
    appDeleteTodo(index) {
      this.todoList.splice(index, 1);
    },
    addTodo(todo) {
      this.todoList.push(todo);
      axios
        .put(
          "https://zeid0012-vue-and-axios.firebaseio.com/data.json",
          this.todoList
        )
        .then(response => {
          console.log("Your data was saved. Status: " + response.status);
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>
<style>
ul {
  list-style: none;
  width: 50%;
  margin: 0% auto;
}
ul li {
  border-bottom: 1px solid #acacac;
  padding: 10px 0;
  margin-bottom: 10px;
}
</style>

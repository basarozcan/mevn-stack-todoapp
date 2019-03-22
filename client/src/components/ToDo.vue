<template lang="html">
  <div>
    <form v-on:submit='ekleToDo($event)'>
      <input type='text' placeholder='Ne yapacaksın?' v-model='yeniToDo'/>
      <input type='submit' />
    </form>
    <ul>
      <li v-for='todo in todos' :key='todo._id'>
        <input type='checkbox' @click='silToDo(todo._id)'> {{todo.title}}
      </li>
    </ul>
  </div>
</template>
<script>
import ToDoAPI from "@/services/ToDoAPI.js";
export default {
  data() {
    return {
      yeniToDo: "",
      todos: []
    };
  },
  mounted() {
    this.loadTodos();
  },
  methods: {
    async ekleToDo(evnt) {
      evnt.preventDefault();
      const response = await ToDoAPI.addTodo(this.yeniToDo);
      this.todos.push(response.data);
      this.yeniToDo = "";
    },
    silToDo (todoID) {
      ToDoAPI.deleteTodo(todoID);
      this.todos = this.todos.filter(function (obj) {
        return obj._id !== todoID;
      })
    },
    async loadTodos() {
      const response = await ToDoAPI.getToDos();
      this.todos = response.data;
    }
  }
};
</script>
<style lang="css">
</style>

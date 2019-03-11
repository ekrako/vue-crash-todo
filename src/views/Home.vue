<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";
import firebase from "../Firebase";
import axios from "axios";
export default {
  name: "home",
  components: {
    AddTodo,
    Todos
  },
  data() {
    return {
      todos: [],
      ref: firebase.firestore().collection("todos")
    };
  },
  methods: {
    deleteTodo(id) {
      console.log(id);
      firebase
        .firestore()
        .collection("todos")
        .doc(id)
        .delete()
        .then(() => (this.todos = this.todos.filter(todo => todo.id !== id)));
    },
    addTodo(newTodo) {
      firebase
        .firestore()
        .collection("todos")
        .add(newTodo);
    }
  },
  created() {
    axios;
    this.ref.onSnapshot(querySnapshot => {
      this.todos = [];
      querySnapshot.forEach(doc => {
        this.todos.push({
          id: doc.id,
          title: doc.data().title,
          completed: doc.data().completed
        });
      });
    });
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background: #666;
}
</style>

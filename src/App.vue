<template>
  <div id="app">
    <Header/>
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Header from './components/layout/Header.vue'
import Todos from './components/Todos.vue';
import AddTodo from './components/AddTodo.vue'
export default {
  name: 'app',
  components: {
    Todos,
    Header,
    AddTodo,
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      fetch(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(res => res.json())
      .then(data =>  this.todos = this.todos.filter(i => i.id !== data.id))
      // eslint-disable-next-line
      .catch(err => console.error(err));
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;

      fetch('https://jsonplaceholder.typicode.com/todos', {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
        },
        body: JSON.stringify({title, completed}),
      })
      .then(res => res.json())
      .then(data => {
        this.todos = [...this.todos, data];
      })
      // eslint-disable-next-line
      .catch(err => console.error(err));
    }
  },
  created() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
    .then(res => res.json())
    .then(data => this.todos = data)
    // eslint-disable-next-line
    .catch(err => console.error(err))
  }
}
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

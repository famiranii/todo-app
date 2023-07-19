<template>
  <todoHeader></todoHeader>

  <main>
    <div class="card add">
      <div class="cb-container">
        <button @click="addBtnClicked" id="add-btn">+</button>
      </div>
      <div class="txt-container">
        <input @keypress.enter="addBtnClicked" v-model="title" type="text" class="txt-input"
          placeholder="Create a new todo..." spellcheck="false" autocomplete="off" />
      </div>
    </div>

    <ul class="todos">
      <addedTodos @deleteItemIsClicked="deleteItemIsClicked" v-for="(todo, index) in todos" :key="index" :todo="todo" />
    </ul>

    <div class="card stat">
      <p class="corner"><span id="items-left">0</span> items left</p>
      <div class="filter">
        <button id="all" class="on">All</button>
        <button id="active">Active</button>
        <button id="completed">Completed</button>
      </div>
      <div class="corner">
        <button @click="clearCompleted" id="clear-completed">Clear Completed</button>
      </div>
    </div>
  </main>
  <todoFooter></todoFooter>
</template>

<script>
import todoHeader from '../src/components/todoHeader.vue';
import todoFooter from '../src/components/todoFooter.vue';
import addedTodos from '../src/components/addedTodos.vue';
import axios from 'axios'
export default {
  components: {
    todoHeader,
    todoFooter,
    addedTodos
  },
  data() {
    return {
      title: '',
      todos: [],
    }
  },
  methods: {
    addBtnClicked() {
      const id = Math.floor(Math.random() * 100)
      const todo = { id: id, title: this.title, isCompleted: false }
      this.todos.push(todo)
      this.title = ''
      axios.post('http://localhost:3000/todos', todo)
        .then(response => {
          console.log(response);
        })
        .catch(error => {
          console.log(error);
        })
    },
    deleteItemIsClicked(id) {
      this.todos = this.todos.filter(todo => todo.id !== id)
      axios.delete(`http://localhost:3000/todos/${id}`)
        .then(response => {
          console.log(response);
        })
        .catch(error => {
          console.error(error.message);
        });
    },
    clearCompleted() {
      var newTodos = this.todos.filter(todo => todo.isCompleted == true)
      var ids = newTodos.map(element => {
        return element.id
      });
      for (let i = 0; i < ids.length; i++) {
        const id = ids[i]
        axios.delete(`http://localhost:3000/todos/${id}`)
          .then(response => {
            console.log(response);
          })
          .catch(error => {
            console.error(error.message);
          });
        this.todos = this.todos.filter(todo => todo.id !== id)
      }
    }

  },
  created() {
    axios.get('http://localhost:3000/todos')
      .then(response => {
        response.data.forEach(element => {
          this.todos.push(element)
        });

      })
      .catch(error => {
        console.error(error);
      });
  },
}
</script>
<style scoped></style>

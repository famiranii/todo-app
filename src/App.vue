<template>
  <todoHeader></todoHeader>

  <main>
    <div class="card add">
      <div class="cb-container">
        <button @click="addBtnClicked" id="add-btn">+</button>
      </div>
      <div class="txt-container">
        <input v-model="title" type="text" class="txt-input" placeholder="Create a new todo..." spellcheck="false"
          autocomplete="off" />
      </div>
    </div>

    <ul class="todos">
      <addedTodos v-for="(todo, index) in todos" :key="index" :todo="todo" />
    </ul>

    <div class="card stat">
      <p class="corner"><span id="items-left">0</span> items left</p>
      <div class="filter">
        <button id="all" class="on">All</button>
        <button id="active">Active</button>
        <button id="completed">Completed</button>
      </div>
      <div class="corner">
        <button id="clear-completed">Clear Completed</button>
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
      counter: 0,
    }
  },
  methods: {
    addBtnClicked() {
      this.counter++
      const todo = { id: this.counter, title: this.title, isCompleted: false }
      this.todos.push(todo)
      this.title = ''
    }
  },
  created() {
    axios.get('http://localhost:3000/todos')
      .then(response => {
        response.data.forEach(element => {
          console.log(element);
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

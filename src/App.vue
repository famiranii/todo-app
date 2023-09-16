<template>
  <todoHeader></todoHeader>

  <main>
    <div class="card add">
      <div class="cb-container"></div>
      <div class="txt-container">
        <input
          @keypress.enter="addBtnClicked"
          v-model="title"
          type="text"
          class="txt-input"
          placeholder="Create a new todo..."
          spellcheck="false"
          autocomplete="off"
        />
      </div>
    </div>

    <ul class="todos">
      <addedTodos
        v-for="(todo, index) in filteredTodo"
        :key="index"
        :todo="todo"
        draggable="true"
        @dragover.prevent
        @dragstart="dragstart(index)"
        @drop="drop(index)"
        @handleCompeleted="handleCompeleted"
        @deleteItemIsClicked="deleteItemIsClicked"
      />
    </ul>

    <div class="card stat">
      <p v-if="isCompletedCount > 1" class="corner">
        <span id="items-left">{{ isCompletedCount }}</span> items left
      </p>
      <p v-else class="corner">
        <span id="items-left">{{ isCompletedCount }}</span> item left
      </p>
      <div class="filter">
        <button :class="{ on: showTodos == 'all' }" @click="this.showTodos = 'all'">All</button>
        <button :class="{ on: showTodos == 'active' }" @click="this.showTodos = 'active'">
          Active
        </button>
        <button :class="{ on: showTodos == 'complete' }" @click="this.showTodos = 'complete'">
          Completed
        </button>
      </div>
      <div class="corner">
        <button @click="clearCompleted" id="clear-completed">Clear Completed</button>
      </div>
    </div>
  </main>
  <todoFooter></todoFooter>
</template>

<script>
import todoHeader from '../src/components/todoHeader.vue'
import todoFooter from '../src/components/todoFooter.vue'
import addedTodos from '../src/components/addedTodos.vue'
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
      draggedTodo: -1,
      showTodos: 'all',
      counter: 0
    }
  },
  computed: {
    isCompletedCount() {
      return this.todos.filter((todo) => todo.isCompleted === false).length
    },
    filteredTodo() {
      if (this.showTodos === 'all') {
        return this.todos
      } else if (this.showTodos === 'active') {
        return this.todos.filter((todo) => todo.isCompleted === false)
      } else {
        return this.todos.filter((todo) => todo.isCompleted === true)
      }
    }
  },
  methods: {
    addBtnClicked() {
      const todo = { title: this.title, isCompleted: false }
      for (let index = 0; index < this.todos.length; index++) {
        const element = this.todos[index].title
        if (element == this.title) {
          return
        }
      }
      if (this.title === '') {
        return
      } else {
        axios
          .post('http://localhost:3000/todos', todo)
          .then((response) => {
            console.log(response)
            this.title = ''
            this.todos=[]
            this.getTodos()
          })
          .catch((error) => {
            console.log(error)
          })
      }
    },
    deleteItemIsClicked(id) {
      console.log(id)
      axios
        .delete(`http://localhost:3000/todos/${id}`)
        .then((response) => {
          console.log(response)
          this.todos = this.todos.filter((todo) => todo.id !== id)
        })
        .catch((error) => {
          console.error(error.message)
        })
    },
    clearCompleted() {
      var newTodos = this.todos.filter((todo) => todo.isCompleted === true)
      var ids = newTodos.map((element) => {
        return element.id
      })
      for (let i = 0; i < ids.length; i++) {
        const id = ids[i]
        axios
          .delete(`http://localhost:3000/todos/${id}`)
          .then((response) => {
            console.log(response)
            this.todos = this.todos.filter((todo) => todo.id !== id)
          })
          .catch((error) => {
            console.error(error.message)
          })
      }
    },
    handleCompeleted(isCompleted, id) {
      console.log(isCompleted, id)
      axios
        .patch(`http://localhost:3000/todos/${id}`, { isCompleted })
        .then((res) => console.log(res))
        .catch((err) => console.log(err))
    },
    dragstart(i) {
      this.draggedTodo = i
    },
    drop(i) {
      var dragged = this.todos.splice(this.draggedTodo, 1)[0]
      this.todos.splice(i, 0, dragged)
    },
    getTodos() {
      axios
        .get('http://localhost:3000/todos')
        .then((response) => {
          this.todos=response.data.reverse()
        })
        .catch((error) => {
          console.error(error)
        })
    }
  },
  created() {
    this.getTodos()
  }
}
</script>
<style scoped></style>

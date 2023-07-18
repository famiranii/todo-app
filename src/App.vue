<template>
  <myHeader></myHeader>
  <main>
    <div class="card add">
      <div class="cb-container">
        <button id="add-btn">+</button>
      </div>
      <div class="txt-container">
        <input type="text" class="txt-input" placeholder="Create a new todo..." spellcheck="false" autocomplete="off" />
      </div>
    </div>
    <ul class="todos">
      <addedTodos v-for="(todo, index) in todos" :key="index" />
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
import myHeader from '../src/components/todoHeader.vue';
import todoFooter from '../src/components/todoFooter.vue';
import addedTodos from '../src/components/addedTodos.vue';
export default {
  components: {
    myHeader,
    todoFooter,
    addedTodos
  },
  data() {
    return {
      todos:[]
    }
  },
}
</script>
<style scoped></style>

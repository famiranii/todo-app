<template>
  <li class="card">
    <div @click="handleCompeleted" class="cb-container cb-btn">
      <input
        v-model="isCompleted"
        :checked="todo.isCompleted ? true : null"
        type="checkbox"
        class="cb-input"
      />
      <div class="check"></div>
    </div>
    <del v-if="todo.isCompleted">{{ todo.title }}</del>
    <span v-else class="item">{{ todo.title }}</span>
    <button @click="deleteItemIsClicked" class="clear">
      <img src="../assets/icon-cross.svg" alt="clear" />
    </button>
  </li>
</template>

<script>
export default {
  props: {
    todo: Object
  },
  data() {
    return {
      isCompleted: this.todo.isCompleted,
      newTodo: this.todo
    }
  },
  methods: {
    deleteItemIsClicked() {
      this.$emit('deleteItemIsClicked', this.todo.id)
    },
    handleCompeleted() {
      this.isCompleted = !this.isCompleted
      this.$emit('handleCompeleted', this.isCompleted, this.todo.id)
    }
  },
  created() {
    console.log(this.todo)
  }
}
</script>

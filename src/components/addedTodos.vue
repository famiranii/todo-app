<template>
    <li class="card">
        <div class="cb-container">
            <input :checked="todo.isCompleted ? true : null" type="checkbox" class="cb-input">
            <div class="check"></div>
        </div>
        <div class="item">{{ todo.title }}</div>
        <button @click="deleteItemIsClicked" class="clear">
            <img src="../assets/icon-cross.svg" alt="clear">
        </button>
    </li>
</template>

<script>
import axios from 'axios'
export default {
    props: {
        todo: Object
    },

    methods: {
        deleteItemIsClicked() {
            axios.delete(`http://localhost:3000/todos/${this.todo.id}`)
                .then(response => {
                    console.log(response);
                    console.log('Item deleted successfully');
                })
                .catch(error => {
                    console.error(error.message);
                });
        }
    }
}
</script>
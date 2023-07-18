<template>
    <li class="card">
        <div class="cb-container">
            <input :checked="todo.isCompleted ? true :null" type="checkbox" class="cb-input">
            <div class="check"></div>
        </div>
        <div class="item">{{ todo.title }}</div>
        <button class="clear">
            <img src="../assets/icon-cross.svg" alt="clear">
        </button>
    </li>
</template>

<script>
export default {
    props: {
        todo: Object
    },

    mounted() {
        console.log(this.todo);
    },
}
</script>
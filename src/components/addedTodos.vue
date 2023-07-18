<template>
    <li class="card">
        <div class="cb-container">
            <input type="checkbox" class="cb-input">
            <div class="check"></div>
        </div>
        <input v-model="title" class="item">
        <button class="clear">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18">
                <path fill="#494C6B" fill-rule="evenodd"
                    d="M16.97 0l.708.707L9.546 8.84l8.132 8.132-.707.707-8.132-8.132-8.132 8.132L0 16.97l8.132-8.132L0 .707.707 0 8.84 8.132 16.971 0z" />
            </svg>
        </button>
    </li>
</template>

<script>
export default {
    props: {
        todos: Object
    },
    data() {
        return {
            title: ''
        }
    },
    watch: {
        title(newTitle) {
            console.log(newTitle);
        },
    },
}
</script>
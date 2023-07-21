<template>
    <header class="card">
        <h1>TODO</h1>
        <button @click="themeSwitcherClicked" id="theme-switcher">
            <img v-if="lightMode" src="../assets/icon-moon.svg" alt="moon">
            <img v-else src="../assets/icon-sun.svg" alt="sun">
        </button>
    </header>
</template>

<script>
export default {
    data() {
        return {
            lightMode: localStorage.getItem('lightMode') === 'true'
        }
    },

    methods: {
        themeSwitcherClicked() {
            this.lightMode = !this.lightMode
            localStorage.setItem('lightMode', this.lightMode)
            const body = document.querySelector('body')
            const root = document.documentElement;
            if (this.lightMode == true) {
                root.style.setProperty('--dark-desaturated-blue', 'hsl(0, 0%, 98%)');
                root.style.setProperty('--very-dark-grayish-blue', 'hsl(233, 11%, 84%)');
                root.style.setProperty('--light-grayish-blue', 'hsl(235, 19%, 35%)');
                root.style.setProperty('--light-blue-hover', 'hsl(235, 19%, 35%)');
                body.classList.add('light')
            } else {
                root.style.removeProperty('--dark-desaturated-blue');
                root.style.removeProperty('--very-dark-grayish-blue');
                root.style.removeProperty('--light-grayish-blue');
                root.style.removeProperty('--light-blue-hover');
                body.classList.remove('light');
            }
        }
    },
    beforeCreate() {
        this.lightMode = localStorage.getItem('lightMode')
        const body = document.querySelector('body')
        const root = document.documentElement;

        if (this.lightMode === 'true') {
            root.style.setProperty('--dark-desaturated-blue', 'hsl(0, 0%, 98%)');
            root.style.setProperty('--very-dark-grayish-blue', 'hsl(233, 11%, 84%)');
            root.style.setProperty('--light-grayish-blue', 'hsl(235, 19%, 35%)');
            root.style.setProperty('--light-blue-hover', 'hsl(235, 19%, 35%)');
            body.classList.add('light')
        } else {
            body.classList.remove('light')
        }
    }
}
</script>
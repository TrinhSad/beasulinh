<template>
    <div id="app">
        <Cover />
    </div>
</template>

<script>
import Cover from './components/Cover.vue'

export default {
    name: 'App',
    components: {
        Cover
    }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&family=Dancing+Script:wght@400;600;700&family=Great+Vibes&display=swap');

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body,
html {
    height: 100%;
    font-family: 'Poppins', sans-serif;
    overflow-x: hidden;
}

#app {
    min-height: 100vh;
    background: linear-gradient(135deg, #ffb6c1, #ffc0cb, #ffb3d1, #ffa0d1);
    background-attachment: fixed;
}
</style>
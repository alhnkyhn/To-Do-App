<template>
  <img alt="Vue logo" src="./assets/logo.png" class="logo">
  <todo-list></todo-list>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
*{
  box-sizing: border-box;
}
.container{
  max-width: 600px;
}
.logo{
  height: 75px;
  margin: 20px auto;
}
</style>

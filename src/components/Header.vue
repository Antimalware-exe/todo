<template>
  <header>
    <div class="header">
      <h1>{{ title }}</h1>
    </div>
    <Login :isUserLoggedIn="isUserLoggedIn" @user-authenticated="authenticateUser" />
    <div class="buttons" v-if="isUserLoggedIn">
      <Button eventName="Add-Task" v-bind:text="showAddTask ? 'Close task' : 'Add task'"
        v-bind:color="showAddTask ? 'red' : 'green'" @Add-Task="handleAddTask" />
    </div>
  </header>
</template>

<script>
import Button from './Button.vue';
import Login from './Login.vue'
export default {
  name: "Header",
  props: {
    title: String,
    showAddTask: Boolean,
    isUserLoggedIn: Boolean
  },
  components: {
    Button, Login
  },
  methods: {
    handleAddTask() {
      this.$emit('toggle-addTask')
    }, authenticateUser(email) {
      this.$emit('user-authenticated', email)
    },
    emits: ['toggle-addTask', 'user-authenticated']
  }
}</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  flex-direction: column;
}

.header {
  display: flex;
  align-items: center;
}

.buttons {
  display: flex;
}
</style>
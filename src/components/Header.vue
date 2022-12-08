<template>
  <header>
    <div class="header">
      <h1>{{ title }}</h1>
    </div>
    <div class="buttons">
      <Button eventName="Add-Task" v-bind:text="showAddTask ? 'Close task' : 'Add task'"
        v-bind:color="showAddTask ? 'red' : 'green'" @Add-Task="handleAddTask" />
    </div>
  </header>
</template>

<script>
import Button from './Button.vue';
export default {
  name: "Header",
  props: {
    title: String,
    showAddTask: Boolean
  },
  components: {
    Button
  },
  methods: {
    handleAddTask() {
      this.$emit('toggle-addTask')
    },
    emits: ['toggle-addTask']
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
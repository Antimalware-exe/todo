<template>
  <div>
    <h3 v-if="incompletedTasks.length > 0" class="status_info">In Progress</h3>
    <div class="tasks" v-for="task in incompletedTasks" v-bind:key="task.id">
      <Task @delete-task="onDelete(task.id)" @toggle-reminder="onReminder(task.id)"
        @complete-toggle="completeToggle(task.id)" @edit-task="onEditTask" v-bind:task="task"
        v-bind:completed="false" />
    </div>

    <h3 v-if="completedTasks.length > 0" class="completed status_info">Completed</h3>
    <div class="completedTasks tasks" v-for="task in completedTasks" v-bind:key="task.id">
      <Task @complete-toggle="completeToggle(task.id)" v-bind:task="task" v-bind:completed="true" />
    </div>
  </div>
</template>

<script>
import Task from './Task.vue'
export default {
  name: "Tasks",
  props: {
    completedTasks: Array,
    incompletedTasks: Array
  },
  components: { Task },
  methods: {
    onDelete(id) { this.$emit("delete-task", id); },
    onReminder(id) { this.$emit("toggle-reminder", id); },
    completeToggle(id) { this.$emit("complete-toggle", id); },
    onEditTask(editedTask) { this.$emit("edit-task", editedTask); }
  },
  emits: ["delete-task", "toggle-reminder", "complete-toggle", "edit-task"]
}</script>

<style>
.status_info {
  text-align: center;
  margin-top: 25px;
  color: #d2691e;
}

.completed {
  border-top: 2px solid grey;
}

.completedTasks {
  text-decoration: line-through;
}
</style>
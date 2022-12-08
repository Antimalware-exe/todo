<template>
  <div v-bind:class="[task.reminder ? 'reminder' : '', 'task']" @dblclick="onToggleReminder">
    <div v-bind:class="[completed ? 'completed' : '']">
      <div class="header">
        <h3 :contenteditable="isEditable" ref="taskText">{{ task.text }}</h3>
        <div class="icons">
          <i class="fas fa-times" v-if="!completed" @click="onDelete"></i>

          <i class="fa fa-pencil" v-if="!completed && !isEditable" @click="onEdit"></i>
          <i class="fa-solid fa-pen-to-square" v-if="(!completed && isEditable)" @click="onEdit"></i>

          <i class="fa fa-check" v-if="!completed" @click="onToggleComplete"></i>
          <i class="fa-solid fa-arrow-up" v-else @click="onToggleComplete"></i>
        </div>
      </div>
      <p :contenteditable="isEditable" ref="taskDay">{{ task.day }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object,
    completed: Boolean
  },
  data() {
    return {
      isEditable: false
    }
  },
  methods: {
    onDelete() {
      this.$emit("delete-task")
    },
    onToggleReminder() {
      this.$emit("toggle-reminder")
      // console.log('Clicked!!!')
    },
    onToggleComplete() {
      this.$emit("complete-toggle")
      // console.log('Clicked!!!')
    },
    onEdit() {
      if (this.isEditable) {
        let taskText = this.$ref.taskText.innerText
        let taskDay = this.$ref.taskDay.innerText
        if (taskText.length > 0 && taskDay.length > 0) {
          const editedTask = { ...this.task, text: taskText, day: taskDay }
          this.$emit('edit-task', editedTask)
        } else {
          // this.$ref.taskText.innerText = this.task.text
          // this.$ref.taskDay.innerText = this.task.day
        }
      }
      this.isEditable = !this.isEditable
    }
  },
  emits: ["delete-task", "toggle-reminder", "complete-toggle", "edit-task"]
}
</script>

<style scoped>
.fas {
  color: red;
}

.fa-check {
  color: green;
  margin-left: 13px;
}

.fa-pencil {
  margin-left: 13px;
  font-size: 13px;
}

.fa-arrow-up {
  margin-left: 13px;
  font-size: 13px;
  color: green;
}

.task {
  background: blanchedalmond;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.reminder {
  border-left: 5px solid green;

}

.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.header {
  display: flex;
  align-items: row;
  justify-content: space-between;
}

.completed {
  text-decoration: line-through;
  text-decoration-thickness: 2px;
  text-decoration-color: red;
  opacity: 0.3;
}
</style>
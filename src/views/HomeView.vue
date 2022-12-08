<template>
  <div class="home">
    <div v-if="showAddTask">
      <AddTask @new-task="createNewTask" />
    </div>
    <Tasks @delete-task="onDelete" @complete-toggle="completeToggle" @toggle-reminder="reminderToggle"
      v-bind:completedTasks='completedTasks' v-bind:incompletedTasks='incompletedTasks' />
  </div>
</template>

<script>
// @ is an alias to /src
import Tasks from '@/components/Tasks.vue'
import AddTask from '@/components/AddTask.vue'

export default {
  name: 'HomeView',
  props: {
    showAddTask: Boolean
  },
  components: {
    Tasks, AddTask
  },

  data() {
    return {
      completedTasks: [],
      incompletedTasks: []
    }
  },

  methods: {
    async fetchTask(id) {
      const res = await fetch(`http://localhost:5001/tasks/${id}`);
      const data = await res.json();
      return data;
    },

    async fetchTasks() {
      const res = await fetch("http://localhost:5001/tasks");
      const data = await res.json();
      return data;
    },

    async onDelete(id) {
      const res = await fetch(`http://localhost:5001/tasks/${id}`, {
        method: "DELETE",
      });
      if (res.status === 200) {
        this.incompletedTasks = this.incompletedTasks.filter(
          (task) => task.id != id
        );
      } else {
        alert("Error deleting data ")
      }
    },

    async completeToggle(id) {
      const taskToComplete = await this.fetchTask(id); // default get
      taskToComplete.completed = !taskToComplete.completed;

      const res = await fetch(`http://localhost:5001/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(taskToComplete),
      });
      const data = await res.json();
      if (data.completed) {
        this.completedTasks = [...this.completedTasks, data];
        this.incompletedTasks = this.incompletedTasks.filter((task) => task.id !== id)
      } else {
        this.incompletedTasks = [...this.incompletedTasks, data];
        this.completedTasks = this.completedTasks.filter((task) => task.id !== id)
      }
    },

    async reminderToggle(id) {
      const reminderOnTask = await this.fetchTask(id); // default get
      reminderOnTask.reminder = !reminderOnTask.reminder;
      const res = await fetch(`http://localhost:5001/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(reminderOnTask),
      });

      const data = await res.json();
      // console.log(data);
      if (data.completed) {
        this.completedTasks.map(ele => ele.id === id ? ele.reminder = data.reminder : ele)
        // this.incompletedTasks = this.incompletedTasks.filter((task) => task.id !== id)
      } else {
        this.incompletedTasks.map(ele => ele.id === id ? ele.reminder = data.reminder : ele)
        // this.completedTasks = this.completedTasks.filter((task) => task.id !== id)
      }
    },
    async createNewTask(newTask) {
      const res = await fetch(`http://localhost:5001/tasks/`, {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(newTask),
      });

      const data = await res.json();
      this.incompletedTasks = [...this.incompletedTasks, data];
      //push method is direct mutable so won't update the array as it creates new memory location for that instance
      // console.log(`Added new task!: ${newTask}`)
    },
  },

  async created() {
    const tasks = await this.fetchTasks();
    this.completedTasks = tasks.filter((task) => task.completed);
    this.incompletedTasks = tasks.filter((task) => !task.completed);
  }
}
</script>

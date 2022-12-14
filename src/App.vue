<template>
  <div class="container">
    <Header title="Task Tracker" v-bind:showAddTask="showAddTask" @toggle-addTask="toggleAddTask"
      :isUserLoggedIn="isUserLoggedIn" @user-authenticated="authenticateUser" />
    <router-view v-bind:showAddTask="showAddTask" v-if="isUserLoggedIn" :userName="userName"></router-view>
    <Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
export default {
  name: "App",
  components: { Header, Footer },
  data() {
    return { userName: '', showAddTask: false, isUserLoggedIn: false }
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask
    },
    authenticateUser(email) {
      this.isUserLoggedIn = !this.isUserLoggedIn
      const encodedEmail = window.btoa(email)
      sessionStorage.setItem('username', encodedEmail)
    }
  },
  async created() {
    let userName = sessionStorage.getItem("username")
    const decodedEmail = window.atob(userName)
    const res = await fetch(`http://localhost:5001/users?email=${decodedEmail}`)
    const user = await res.json()
    console.log(decodedEmail)
    if (user.length === 1) {
    
      this.userName = decodedEmail
      this.isUserLoggedIn = true;
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Poppins", sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>

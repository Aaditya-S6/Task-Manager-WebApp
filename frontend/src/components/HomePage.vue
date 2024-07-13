<template>
  <header>
    <span>Task Manager <sup>TM</sup></span>
    <nav>
      <router-link v-if="isAuthenticated" to="/list">TaskPage</router-link>

      <router-link v-if="!isAuthenticated" to="/signup">Signup</router-link>

      <router-link v-if="isAuthenticated" @click="logout" to="#"
        >Logout</router-link
      >

      <router-link v-else to="/login">Login</router-link>
    </nav>
  </header>

  <div class="center-wrap">
    <h2  v-if="!isAuthenticated">Hey!👋 <br> Welcome to Task Manager</h2>
    <p v-if="!isAuthenticated">
          Seamlessly manage your tasks 
    </p>
    <h2 v-if="isAuthenticated">Hey {{ getUserName }}, <br> You can Add your Task !</h2>

    <!-- Task creation form -->
    <form v-if="isAuthenticated" @submit.prevent="createTask">
      <label for="username">Enter your task:</label>
      <input type="text" v-model="taskText" required />
      <label for="username">Enter task description:</label>
      <div><textarea v-model="description" rows="4" required></textarea></div>
      <button type="submit">Create</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HomePage",
  data() {
    return {
      taskText: "",
      description: ""
    };
  },
  computed: {
    isAuthenticated() {
      // Check if the user is authenticated, e.g., by checking the presence of JWT token
      return localStorage.getItem("jwtToken") !== null;
    },
    getUserId() {
      // Extract user ID from the JWT token
      const token = localStorage.getItem("jwtToken");
      if (token) {
        const decodedToken = JSON.parse(atob(token.split(".")[1]));
        return decodedToken.userId;
      }
      return null;
    },
    getUserName() {
      const token = localStorage.getItem("jwtToken");
      if (token) {
        const decodedToken = JSON.parse(atob(token.split(".")[1]));
        return decodedToken.username;
      }
      return null;
    },
  },
  methods: {
    logout() {
      // Clear session storage
      localStorage.removeItem("jwtToken");

      // Redirect to the login route
      this.$router.push({ name: "Login" });
    },
    createTask() {
      // Example using axios (make sure to install axios first: npm install axios)
      axios
        .post("http://localhost:3000/tasks/create", {
          taskName: this.taskText,
          description: this.description,
          userId: this.getUserId,
        })
        .then(() => {
          // Placeholder alert (remove this in your actual implementation)
          alert("Task created successfully");

          // Redirect to the login route
          this.$router.push({ name: "List" });
        })
        .catch((error) => {
          // Check if the error response contains a message
          if (
            error.response &&
            error.response.data &&
            error.response.data.message
          ) {
            // Display the error message as an alert
            alert(error.response.data.message);
          } else {
            // Default error message if no specific message is available
            alert(
              "Error logging in. Please check your credentials and try again."
            );
          }
        });

      // Clear the input field after creating the task
      this.taskText = "";
      this.description = "";
    },
  },
};
</script>

<style scoped>
header {
  background-color: #6e0218c9;
  display: flex;
  padding: 5px;
  justify-content: space-between;
  color: #fffefe;
  text-align: center;
}
form{
  display: flex;
  flex-direction: column;
  width: fit-content;
  border: 2px solid #7a0008;
  background-color: #dd8086;
  padding: 10px;
  border-radius: 20px;

}

header span {
  font-size: 1.2rem;
  text-transform: uppercase;
  font-weight: bold;
}
nav {
  display: flex;
}
nav a {
  float: right;
  margin-right: 15px;
  font-size: 1.2rem;
  color: #fff;
  text-decoration: none;
}
nav a:hover {
  text-decoration: underline;
}

.center-wrap {
  width: 100%;
  height: 90vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

h1 {
  font-size: 2.5rem;
  font-weight: bold;
}
p {
  margin: 0;
  padding: 0;
  font-size: 1.2rem;
}

form label {
  font-size: 1.1rem;
}

form input,
textarea {
  width: 100%;
  display: flex;
  margin-bottom: 10px;
  padding: 0.5rem;
  text-align: start;
  font-size: 1.1rem;
}

form select {
  position: relative;
  left: 74px;
  weight: 100%;
  padding: 5px;
  margin-bottom: 5px;
}

form button {
  width: 100%;
  padding: 0.5rem 2rem;
  background-color: #6a0108;
  color: white;
  font-size: 0.8rem;
  border: 0;
  cursor: pointer;
  margin:10px;
  border-radius:10px;
  align-self: center;
}
nav a{
  border: 1px solid white;
  border-radius:10px ;
  padding: 2px 5px;
}
</style>

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
  <div class="login-wrap">
    <h2>Login Page</h2>
    <form @submit.prevent="login">
      <label for="username">Username:</label>
      <input v-model="loginForm.username" type="text" id="username" required />

      <label for="password">Password:</label>
      <input
        v-model="loginForm.password"
        type="password"
        id="password"
        required
      />

      <button type="submit">Login</button>
    </form>
    <router-link to="/">Back to Home</router-link>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "LoginPage",
  data() {
    return {
      loginForm: {
        username: "",
        password: "",
      },
    };
  },
  methods: {
    login() {
      const userData = {
        username: this.loginForm.username,
        password: this.loginForm.password,
      };

      axios
        .post("http://localhost:3000/users/signin", userData)
        .then((response) => {
          // Clear input fields
          this.loginForm.username = "";
          this.loginForm.password = "";

          // Store JWT token in localStorage
          localStorage.setItem("jwtToken", response.data.token);

          // Show success alert
          alert("User logged in successfully!");

          // Redirect to the homepage
          this.$router.push({ path: "/" });
        })
        .catch((error) => {
          console.error("Error logging in", error);

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
    },
  },
};
</script>

<style scoped>
.login-wrap {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

form label {
  font-size: 1.1rem;
}

form input {
  display: flex;
  margin-bottom: 10px;
  padding: 0.5rem;
  text-align: start;
  font-size: 1.1rem;
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

form{
  display: flex;
  flex-direction: column;
  width: fit-content;
  border: 2px solid #7a0008;
  background-color: #dd8086;
  padding: 10px;
  border-radius: 20px;

}


.login-wrap a {
  padding-top: 0.5rem;
  color: #03375a;
  font-size: 1rem;
  cursor: pointer;
  text-decoration: none;
  text-emphasis: bold;
  font-weight: bolder;
}

header {
  background-color: #6e0218c9;
  display: flex;
  padding: 5px;
  justify-content: space-between;
  color: #fffefe;
  text-align: center;
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
nav a{
  border: 1px solid white;
  border-radius:10px ;
  padding: 2px 5px;
}
</style>

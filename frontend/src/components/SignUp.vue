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
  <div class="signup-wrap">
    <h1>Signup Page</h1>
    <form @submit.prevent="signup">
      <label for="username">Username:</label>
      <input v-model="signupForm.username" type="text" id="username" required />

      <label for="password">Password:</label>
      <input
        v-model="signupForm.password"
        type="password"
        id="password"
        required
      />

      <label for="confirmPassword">Confirm Password:</label>
      <input
        v-model="signupForm.confirmPassword"
        type="password"
        id="confirmPassword"
        required
      />

      <button  type="submit">Sign Up</button>
    </form>

    <router-link to="/login">Already have an Account? Login</router-link>
    <router-link to="/">Go to Home</router-link>


  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "SignupPage",
  data() {
    return {
      signupForm: {
        username: "",
        password: "",
        confirmPassword: "",
      },
    };
  },
  methods: {
    signup() {
      if (this.signupForm.password !== this.signupForm.confirmPassword) {
        alert("Passwords do not match. Please confirm your password.");
        return;
      }
      const userData = {
        username: this.signupForm.username,
        password: this.signupForm.password,
      };

      axios
        .post("http://localhost:3000/users/create", userData)
        .then(() => {
          // Clear input fields
          this.signupForm.username = "";
          this.signupForm.password = "";
          this.signupForm.confirmPassword = "";

          // Show success alert
          alert("User registered successfully!");
        })
        .catch((error) => {
          alert("Error registering user", error);
          // Handle registration error, show an error message, etc.
        });
    },
  },
};
</script>

<style scoped>
.signup-wrap {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
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

.signup-wrap a {
  padding-top: 0.5rem;
  color: #034a79;
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

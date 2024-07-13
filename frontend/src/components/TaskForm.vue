<template>
      <!-- Task creation form -->
      <form v-if="isAuthenticated" @submit.prevent="createTask">
        <label for="username">Enter your task:</label>
        <input type="text" v-model="taskText" required />
        <label for="username">Enter task description:</label>
        <div><textarea v-model="description" rows="4" required></textarea></div>
        <button type="submit">Create</button>
      </form>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    name: "TaskForm",
    data() {
      return {
        taskText: "",
        description: ""
      };
    },
    methods: {
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
  
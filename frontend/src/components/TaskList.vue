<template>
  <div>
    <h1>Task List Page</h1>
    <p>Details of your Tasks</p>


    <!-- Display the list of tasks for the user in a table -->
    <table v-if="filteredTasks.length > 0">
      <thead>
        <tr>
          <th>Sr. No</th>
          <th>Task Name</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in filteredTasks" :key="task._id">
          <td>{{ index + 1 }}</td>
          <td>{{ task.taskName }}</td>
          <td>{{ task.description }}</td>
        </tr>
      </tbody>
    </table>
    <p v-else>Oops..! You have not created any task yet.</p>
    <p>
      <router-link to="/">Back to Home</router-link>
    </p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ListPage',
  data() {
    return {
      tasks: []
    };
  },
  created() {
    // Fetch tasks for the user when the component is created
    this.fetchTasks();
  },
  computed: {
    filteredTasks() {
      // Filter tasks based on selected priority
      return this.tasks;
    },
  },
  methods: {
    async fetchTasks() {
      try {
        // Extract JWT token from localStorage
        const token = localStorage.getItem('jwtToken');

        // Check if token is available
        if (!token) {
          console.error('JWT token not found in localStorage.');
          return;
        }

        // Decode the token to get the payload
        const decodedToken = JSON.parse(atob(token.split('.')[1]));

        // Extract user ID from the decoded token
        const userId = decodedToken.userId;

        // Make a GET request to fetch tasks for the user
        const response = await axios.get(
          `http://localhost:3000/tasks/${userId}`,
        );

        // Update the tasks data property with the fetched tasks
        this.tasks = response.data;
      } catch (error) {
        alert('Error fetching tasks', error);
      }
    },
  },
};
</script>

<style scoped>
h1,
p {
  text-align: center;
}
a {
  color: #002f7a;
  font-size: 1rem;
  cursor: pointer;
  text-decoration: none;
  padding: 0.5rem 1rem;
  text-decoration: none;
  text-emphasis: bold;
  font-weight: bolder;
  /* background-color: antiquewhite;
  border-radius: 10px; */
}

table {
  width: 90%;
  border-collapse: collapse;
  margin-top: 1rem;
  align-self: center;
}

th,
td {
  border: 1px solid antiquewhite;
  padding: 8px;
  text-align: left;
  background-color:#ecc2c5; 

}

th {
  background-color: #7a0008;
  color: white;
}

div {
  margin-bottom: 1rem;
  display: flex;
  flex-direction: column;
}
</style>

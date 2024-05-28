<template>
  <div class="employee-list">
    <div class="employee" v-for="employee in employees" :key="employee.id">
      <img :src="employee.avatar" alt="Profile Picture" class="profile-pic" />
      <div class="employee-details">
        <h3>{{ employee.first_name }} {{ employee.last_name }}</h3>
        <a :href="'mailto:' + employee.email">Contact</a>
      </div>
    </div>
    <div class="pagination">
      <button
        @click="fetchEmployees(currentPage - 1)"
        :disabled="currentPage === 1"
      >
        Previous
      </button>
      <button
        @click="fetchEmployees(currentPage + 1)"
        :disabled="currentPage === totalPages"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      employees: [],
      currentPage: 1,
      totalPages: 1,
    };
  },
  created() {
    this.fetchEmployees(this.currentPage);
  },
  methods: {
    async fetchEmployees(page) {
      if (page < 1 || page > this.totalPages) return;
      try {
        const response = await axios.get(
          `https://reqres.in/api/users?page=${page}`
        );
        this.employees = response.data.data;
        this.currentPage = response.data.page;
        this.totalPages = response.data.total_pages;
      } catch (error) {
        console.error("Error fetching employees:", error);
      }
    },
  },
};
</script>

<style scoped>
.employee-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 1em;
  background: rgb(255, 255, 255);
  background: linear-gradient(
    180deg,
    rgba(204, 195, 230, 0.687) 32%,
    rgba(255, 255, 255, 1) 100%
  );
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  padding-top: 30px;
  margin: auto;
}

.employee {
  display: flex;
  align-items: center;
  margin: 1em 0;
  padding: 1em;
  border: solid 2px #5333ed;
  background-color: rgba(240, 248, 255, 0.435);
  border-radius: 8px;
  width: 100%;
  max-width: 600px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.profile-pic {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  margin-right: 1em;
  object-fit: cover;
}

.employee-details {
  display: flex;
  flex-direction: column;
}

.employee-details h3 {
  font-size: 20px;
  margin: 0;
}

.employee-details a {
  color: #007bff;
  text-decoration: none;
  margin-top: 0.5em;
  font-size: large;
}

.employee-details a:hover {
  color: rgb(189, 55, 122);
}

.pagination {
  display: flex;
  justify-content: space-between;
  width: 100%;
  max-width: 600px;
  margin-top: 1em;
}

button {
  padding: 0.5em 1em;
  border: none;
  border-radius: 4px;
  background-color: #5333ed;
  color: white;
  cursor: pointer;
  font-size: large;
  width: 17%;
  height: 40px;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

@media (max-width: 600px) {
  .employee-list {
    padding:20px 10%;
  }
  .employee {
    flex-direction: column;
    align-items: center;
  }

  .profile-pic {
    margin-bottom: 1em;
  }

  .employee-details {
    align-items: center;
    text-align: center;
  }

  .pagination {
    flex-direction: column;
    align-items: center;
  }

  button {
    width: 100%;
    max-width: 200px;
    margin: 0.5em 0;
  }
}
</style>

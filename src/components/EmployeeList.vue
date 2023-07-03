<template>
  <div class="employee-list">
    <h1>Employee List</h1>
    <div v-if="loading">Loading...</div>
    <div v-else class="employee-wrapper">
      <div v-for="employee in employees" :key="employee.id" class="employee">
        <img :src="employee.avatar" :alt="employee.first_name" />
        <p>{{ employee.first_name }} {{ employee.last_name }}</p>
        <a :href="`mailto:${employee.email}`">{{ employee.email }}</a>
      </div>
    </div>
    <div id="pagination-buttons">
        <button @click="previousPage" :disabled="page === 1">Previous</button>
        <button @click="nextPage" :disabled="page === totalPages">Next</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      employees: [],
      page: 1,
      totalPages: 0,
      loading: true,
    };
  },
  mounted() {
    this.fetchEmployees();
  },
  methods: {
    async fetchEmployees() {
      try {
        const API_URL = 'https://reqres.in/api/users';
        const response = await fetch(`${API_URL}?page=${this.page}`);
        const data = await response.json();
        this.employees = data.data;
        this.totalPages = data.total_pages;
      } catch (error) {
        console.error("Error fetching employees:", error);
      } finally {
        this.loading = false;
      }
    },
    previousPage() {
      if (this.page > 1) {
        this.page--;
        this.fetchEmployees();
      }
    },
    nextPage() {
      if (this.page < this.totalPages) {
        this.page++;
        this.fetchEmployees();
      }
    },
  },
};
</script>
<style lang="scss">
.employee-list {
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh; /* Adjust the height as needed */
  width:100%;

  h1 {
    text-align:center;
    font-size: 24px;
    color: #333;
    margin-bottom: 4rem;
  }

  .loading {
    font-weight: bold;
    color: #666;
  }
  .employee-wrapper
  {
    display:flex;
    flex-direction:row;
    gap:20px;
    flex-wrap:wrap;
    align-items: center;
    justify-content: center;
  }

  .employee {
    padding: 5px;
    background-color: #f5f5f5;
    margin-bottom: 5px;
    border-radius:10px;
    padding:20px;
    display:flex;
    align-items:center;
    flex-direction:column;
    gap:8px;
    width:200px;


    img {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      margin-right: 10px;
    }

    p {
      margin: 0;
    }

    &:hover {
      opacity:0.8;
    }
  }

  #pagination-buttons {
    margin-top: 10px;

    button {
      margin-right: 5px;
      padding: 5px 10px;
      background-color:#47515b;
      color:white;
      font-weight:bolder;
      border-radius:6px;
      border:none;

      &:hover {
        cursor: pointer;
        opacity:0.8;
      }
    }
  }
}
</style>

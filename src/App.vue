<template>
  <div id="app">
    <h1>Employee CRUD Application</h1>
    <EmployeeForm
      :employeeToEdit="employeeToEdit"
      @employeeSaved="handleEmployeeSaved"
    />
    <EmployeeList
      :employees="employees"
      @fetchEmployees="fetchEmployees"
      @editEmployee="setEmployeeToEdit"
    />
  </div>
</template>

<script>
import EmployeeList from "./components/EmployeeList.vue";
import EmployeeForm from "./components/EmployeeForm.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    EmployeeList,
    EmployeeForm,
  },
  data() {
    return {
      employees: [],
      employeeToEdit: null,
    };
  },
  methods: {
    fetchEmployees() {
      axios
        .get("http://localhost:8080/api/employees")
        .then((response) => {
          this.employees = response.data;
        })
        .catch((error) => console.error(error));
    },
    setEmployeeToEdit(employee) {
      this.employeeToEdit = employee;
    },
    handleEmployeeSaved() {
      this.fetchEmployees();
      this.employeeToEdit = null; // Reset edit mode
    },
  },
  mounted() {
    this.fetchEmployees();
  },
};
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin: 20px auto;
  color: #333;
}
h1 {
  color: #007bff;
  margin-bottom: 20px;
}
</style>

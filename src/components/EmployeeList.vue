<template>
    <div>
      <h2>Employee List</h2>
      <table border="1" cellpadding="10">
        <thead>
          <tr>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Email</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="employee in employees" :key="employee.id">
            <td>{{ employee.firstName }}</td>
            <td>{{ employee.lastName }}</td>
            <td>{{ employee.email }}</td>
            <td>
              <button @click="editEmployee(employee)">Edit</button>
              <button @click="deleteEmployee(employee.id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    name: "EmployeeList",
    props: ["employees"],
    methods: {
      editEmployee(employee) {
        this.$emit("editEmployee", employee);
      },
      deleteEmployee(id) {
        axios
          .delete(`http://localhost:8080/api/employees/${id}`)
          .then(() => {
            this.$emit("fetchEmployees");
          })
          .catch((error) => console.error(error));
      },
    },
  };
  </script>
<style scope>
</style>  
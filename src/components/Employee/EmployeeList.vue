<template>
    <div class="employee-list">
      <h2>Employee List</h2>
      <table>
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
  
  <style scoped>
  .employee-list {
    width: 80%;
    margin: 20px auto;
  }
  .employee-list h2 {
    text-align: center;
    margin-bottom: 20px;
  }
  .employee-list table {
    width: 100%;
    border-collapse: collapse;
  }
  .employee-list th,
  .employee-list td {
    text-align: left;
    padding: 10px;
    border: 1px solid #ddd;
  }
  .employee-list th {
    background-color: #f4f4f4;
  }
  .employee-list button {
    padding: 5px 10px;
    margin: 0 5px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  .employee-list button:hover {
    opacity: 0.8;
  }
  button:nth-child(1) {
    background-color: #007bff;
    color: white;
  }
  button:nth-child(2) {
    background-color: #dc3545;
    color: white;
  }
  </style>
  
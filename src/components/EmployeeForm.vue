<template>
    <div class="employee-form">
      <h2>{{ isEditMode ? "Edit Employee" : "Add New Employee" }}</h2>
      <form @submit.prevent="submitForm">
        <label>
          First Name:
          <input type="text" v-model="employee.firstName" required />
        </label>
        <label>
          Last Name:
          <input type="text" v-model="employee.lastName" required />
        </label>
        <label>
          Email:
          <input type="email" v-model="employee.email" required />
        </label>
        <button type="submit">{{ isEditMode ? "Update" : "Add" }} Employee</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    name: "EmployeeForm",
    props: ["employeeToEdit"],
    data() {
      return {
        employee: {
          firstName: "",
          lastName: "",
          email: "",
        },
        isEditMode: false,
      };
    },
    watch: {
      employeeToEdit: {
        immediate: true,
        handler(newVal) {
          if (newVal) {
            this.isEditMode = true;
            this.employee = { ...newVal }; // Populate form for editing
          } else {
            this.isEditMode = false;
            this.resetForm();
          }
        },
      },
    },
    methods: {
      submitForm() {
        const endpoint = "http://localhost:8080/api/employees";
        const request = this.isEditMode
          ? axios.put(`${endpoint}/${this.employee.id}`, this.employee)
          : axios.post(endpoint, this.employee);
  
        request
          .then(() => {
            this.$emit("employeeSaved");
            this.resetForm();
          })
          .catch((error) => console.error(error));
      },
      resetForm() {
        this.employee = { firstName: "", lastName: "", email: "" };
      },
    },
  };
  </script>
  
  <style scoped>
  .employee-form {
    max-width: 400px;
    margin: 20px auto;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: #f9f9f9;
  }
  .employee-form h2 {
    text-align: center;
    margin-bottom: 20px;
  }
  .employee-form label {
    display: block;
    margin-bottom: 10px;
    font-weight: bold;
  }
  .employee-form input {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  .employee-form button {
    display: block;
    width: 100%;
    padding: 10px;
    background-color: #28a745;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  .employee-form button:hover {
    background-color: #218838;
  }
  </style>
  
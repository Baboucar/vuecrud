<template>
    <div>
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
  
<template>
  <div id="app">
   <div class="employee">
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
  <div class="movie">
    <h1>Movie CRUD Application</h1>
    <MovieForm
      :movieToEdit="movieToEdit"
      @movieSaved="handleMovieSaved"
    />
    <MovieList
      :movies="movies"
      @fetchMovies="fetchMovies"
      @editMovie="setMovieToEdit"
    />
  </div>
  </div>
</template>

<script>
import EmployeeList from "./components/Employee/EmployeeList.vue";
import EmployeeForm from "./components/Employee/EmployeeForm.vue";
import MovieForm from "./components/Movie/MovieForm.vue";
import MovieList from "./components/Movie/MovieList.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    EmployeeList,
    EmployeeForm,
    MovieForm,
    MovieList,
  },
  data() {
    return {
      employees: [],
      employeeToEdit: null,
      movies: [],
      movieToEdit: null,
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
    fetchMovies() {
      axios
        .get("http://localhost:8080/api/movies")
        .then((response) => {
          this.movies = response.data;
        })
        .catch((error) => console.error(error));
    },
    setMovieToEdit(movie) {
      this.movieToEdit = movie;
    },
    handleMovieSaved() {
      this.fetchMovies();
      this.movieToEdit = null; // Reset edit mode
    },
  },
  mounted() {
    this.fetchEmployees();
    this.fetchMovies();
  },
};
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin: 20px auto;
  color: #b99a9a;
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100vw;
}
h1 {
  color: #007bff;
  margin-bottom: 20px;
}
.employee{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.movie{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>

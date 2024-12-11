<template>
    <div class="employee-list">
      <h2>Employee List</h2>
      <table>
        <thead>
          <tr>
            <th>Title</th>
            <th>Description</th>
            <th>Quality</th>
            <th>Genre</th>
            <th>Duration</th>
            <th>Release Date</th>
            <th>Average Rating</th>
            <th>View Count</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="movie in movies" :key="movie.id">
            <td>{{ movie.title }}</td>
            <td>{{ movie.movieDescription }}</td>
            <td>{{ movie.quality }}</td>
            <td>{{ movie.genre }}</td>
            <td>{{ movie.duration }}</td>
            <td>{{ movie.releaseDate }}</td>
            <td>{{ movie.avrRating }}</td>
            <td>{{ movie.viewCount }}</td>

            <td>
              <button @click="editMovie(movie)">Edit</button>
              <button @click="deleteMovie(movie.id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    name: "MovieList",
    props: ["movies"],
    methods: {
      editMovie(movie) {
        this.$emit("editMovie", movie);
      },
      deleteMovie(id) {
        axios
          .delete(`http://localhost:8080/api/movies/${id}`)
          .then(() => {
            this.$emit("fetchMovies");
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
  
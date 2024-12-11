<template>
  <div class="movie-form">
    <h2>{{ isEditMode ? "Edit Movie" : "Add New Movie" }}</h2>
    <form @submit.prevent="submitForm">
      <label>
        Title:
        <input type="text" v-model="movie.title" required />
      </label>
      <label>
        Description:
        <input type="text" v-model="movie.movieDescription" required />
      </label>
      <label for="quality">Choose the quality of the movie:</label>
      <select v-model="movie.quality" name="quality" id="quality">
        <option value="HD">HD</option>
        <option value="CAM">CAM</option>
      </select>
      <label for="genre">Choose genre of the movie:</label>
      <select v-model="movie.genre" name="genre" id="genre">
        <option value="HORROR">HORROR</option>
        <option value="ACTION">ACTION</option>
        <option value="ANIME">ANIME</option>
        <option value="ROMANCE">ROMANCE</option>
        <option value="DRAMA">DRAMA</option>
        <option value="COMEDY">COMEDY</option>
        <option value="THRILLER">THRILLER</option>
      </select>
      <label>
        Duration:
        <input type="text" v-model="movie.duration" required />
      </label>
      <label>
        Release Date:
        <input type="date" v-model="movie.releaseDate" required />
      </label>
      <label>
        Average Rating:
        <input type="number" v-model="movie.avrRating" required />
      </label>
      <label>
        View Count:
        <input type="text" v-model="movie.viewCount" required />
      </label>
      <button type="submit">{{ isEditMode ? "Update" : "Add" }} Movie</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "MovieForm",
  props: ["movieToEdit"],
  data() {
    return {
      movie: {
        title: "",
        movieDescription: "",
        quality: "",
        genre: "",
        duration: "",
        releaseDate: "",
        avrRating: "",
        viewCount: ""
      },
      isEditMode: false,
    };
  },
  watch: {
    movieToEdit: {
      immediate: true,
      handler(newVal) {
        if (newVal) {
          this.isEditMode = true;
          this.movie = { ...newVal }; // Populate form for editing
        } else {
          this.isEditMode = false;
          this.resetForm();
        }
      },
    },
  },
  methods: {
    submitForm() {
      const endpoint = "http://localhost:8080/api/movies";
      const request = this.isEditMode
        ? axios.put(`${endpoint}/${this.movie.id}`, this.movie)
        : axios.post(endpoint, this.movie);

      request
        .then(() => {
          this.$emit("movieSaved");
          this.resetForm();
        })
        .catch((error) => console.error(error));
    },
    resetForm() {
      this.movie = {
        title: "",
        movieDescription: "",
        quality: "",
        genre: "",
        duration: "",
        releaseDate: "",
        avrRating: "",
        viewCount: ""
      };
    },
  },
};
</script>

<style scoped>
.movie-form {
  max-width: 400px;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
}

.movie-form h2 {
  text-align: center;
  margin-bottom: 20px;
}

.movie-form label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
}

.movie-form input {
  width: 100%;
  padding: 8px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.movie-form button {
  display: block;
  width: 100%;
  padding: 10px;
  background-color: #28a745;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.movie-form button:hover {
  background-color: #218838;
}
</style>
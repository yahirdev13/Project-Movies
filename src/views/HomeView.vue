<template>
  <div class="container">
    <div class="mt-3 me-3 d-flex justify-content-end">
      <b-button variant="outline-primary" @click="showModal">Agregar pelicula</b-button>
    </div>

    <div class="row mt-3">
      <div class="col-md-4" v-for="(movie, index) in movies" :key="index">
        <b-card :title="movie.title" class="mb-3 ms-3 me-3">
          <p class="card-text">Director: {{ movie.director }}</p>
          <p class="card-text">Duración: {{ movie.duration }}</p>
          <p class="card-text">Género: {{ movie.genre }}</p>
          <b-button variant="danger" size="sm" @click="confirmDelete(index)">Eliminar</b-button>
          <b-button variant="warning" size="sm" class="ms-2" @click="editMovie(index)">Modificar</b-button>
        </b-card>
      </div>
    </div>

    <b-modal v-model="modalShow" title="Agregar nueva película">
      <form @submit.prevent="addMovie">
        <b-form-group id="movieTitle" label="Título:" label-for="title">
          <b-form-input id="title" v-model="newMovie.title" required></b-form-input>
        </b-form-group>
        <b-form-group id="movieDirector" label="Director:" label-for="director">
          <b-form-input id="director" v-model="newMovie.director" required></b-form-input>
        </b-form-group>
        <b-form-group id="movieDuration" label="Duración:" label-for="duration">
          <b-form-input id="duration" v-model="newMovie.duration" required></b-form-input>
        </b-form-group>
        <b-form-group id="movieGenre" label="Género:" label-for="genre">
          <b-form-input id="genre" v-model="newMovie.genre" required></b-form-input>
        </b-form-group>
        <b-button type="submit" variant="primary" class="mt-3">Agregar</b-button>
      </form>
    </b-modal>


  </div>
</template>

<script>
export default {
  data() {
    return {
      movies: [
        { title: 'Pelicula 1', director: 'Director 1', duration: '120 min', genre: 'Acción' },
        { title: 'Pelicula 2', director: 'Director 2', duration: '110 min', genre: 'Comedia' },
        { title: 'Pelicula 3', director: 'Director 3', duration: '100 min', genre: 'Drama' },
        { title: 'Pelicula 4', director: 'Director 4', duration: '130 min', genre: 'Aventura' },
        { title: 'Pelicula 5', director: 'Director 5', duration: '115 min', genre: 'Ciencia ficción' },
        { title: 'Pelicula 6', director: 'Director 6', duration: '125 min', genre: 'Romance' },
        { title: 'Pelicula 7', director: 'Director 7', duration: '105 min', genre: 'Suspense' },
        { title: 'Pelicula 8', director: 'Director 8', duration: '135 min', genre: 'Fantasía' },
        { title: 'Pelicula 9', director: 'Director 9', duration: '95 min', genre: 'Animación' },
        { title: 'Pelicula 10', director: 'Director 10', duration: '140 min', genre: 'Misterio' }
      ],
      modalShow: false,
      newMovie: {
        title: '',
        director: '',
        duration: '',
        genre: ''
      }
    };
  },
  methods: {
    showModal() {
      this.modalShow = true;
    },
    addMovie() {
      // Agregar la nueva película al array de películas
      this.movies.push({
        title: this.newMovie.title,
        director: this.newMovie.director,
        duration: this.newMovie.duration,
        genre: this.newMovie.genre
      });

      // Limpiar los campos del formulario y cerrar el modal
      this.newMovie.title = '';
      this.newMovie.director = '';
      this.newMovie.duration = '';
      this.newMovie.genre = '';
      this.modalShow = false;
    },
    confirmDelete(index) {
      if (window.confirm('¿Estás seguro de que quieres eliminar esta película?')) {
        this.deleteMovie(index);
      }
    },
    deleteMovie(index) {
      // Eliminar película del array de películas
      this.movies.splice(index, 1);
    },
    editMovie(index) {
      // Asignar los datos de la película seleccionada a newMovie
      const selectedMovie = this.movies[index];
      this.newMovie = {
        title: selectedMovie.title,
        director: selectedMovie.director,
        duration: selectedMovie.duration,
        genre: selectedMovie.genre
      };

      // Abrir el modal
      this.modalShow = true;
    },
    clearFields() {
      this.newMovie.title = '';
      this.newMovie.director = '';
      this.newMovie.duration = '';
      this.newMovie.genre = '';
      this.modalShow = false;
    }

  }
};
</script>

<style scoped>
.container {
  max-width: 100%;
  /* Elige el ancho máximo deseado */
  overflow-x: hidden;
  /* Ocultar cualquier contenido que sobresalga del contenedor */
}
</style>
<template>
  <div class="peliculas-container">
    <h3>PELICULAS EN CATALOGO</h3>
    <div v-show="showElement" class="drop-zone" @drop="onDrop($event)" @dragover.prevent @dragenter.prevent>
      <p>Arrastra hasta aca el formulario lleno para agregar una nueva pelicula</p>
    </div>
    <div class="row">
      <div v-show="peliculas === null" class="text-center">
        <h4>No hay películas registradas </h4>
      </div>
      <div class="col-md-2 mt-3" v-for="pelicula in peliculas" :key="pelicula.idMovie">
        <div class="card pelicula-card">
          <div class="card-body">
            <h5 class="card-title">{{ pelicula.name }}</h5>
            <p class="card-text">Director: {{ pelicula.director }}</p>
            <p class="card-text">Género: {{ pelicula.genre.name }}</p>
            <p class="card-text">Fecha de estreno:
              {{ pelicula.releaseDate.split('T')[0] }}</p>
            <p class="card-text">Duración: {{ pelicula.duration }} minutos</p>
            <div class="d-flex justify-content-between">
              <b-button @click="getPelicula(pelicula.idMovie)" v-b-modal.modal-prevent-closing
                variant="warning">Actualizar</b-button>
              <b-button @click="deleteMovie(pelicula.idMovie)" type="button" variant="danger">Eliminar</b-button>
            </div>
          </div>
        </div>
      </div>

      <div class="drag-el" draggable @dragstart="startDrag($event)">
        <div class="form-container">
          <h4>Registrar película</h4>
          <form ref="form" @submit.stop.prevent="handleSubmit">
            <div class="form-group">
              <label for="name-input">Nombre</label>
              <input id="name-input" class="form-control" v-model="peliculaCreate.name" required>
              <div class="invalid-feedback">El nombre es requerido</div>
            </div>
            <div class="form-group">
              <label for="director-input">Director</label>
              <input id="director-input" class="form-control" v-model="peliculaCreate.director" required>
              <div class="invalid-feedback">El director es requerido</div>
            </div>
            <div class="form-group">
              <label for="genre-input">Género</label>
              <select id="genre-input" class="form-control" v-model="peliculaCreate.genre" required>
                <option disabled value="">Selecciona un género</option>
                <option v-for="genre in genres" :key="genre.idGenre" :value="genre.idGenre">{{ genre.name }}</option>
              </select>
              <div class="invalid-feedback">El género es requerido</div>
            </div>
            <div class="form-group">
              <label for="duration-input">Duración (minutos)</label>
              <input id="duration-input" class="form-control" v-model="peliculaCreate.duration" type="number" required>
              <div class="invalid-feedback">La duración es requerida</div>
            </div>
            <div class="form-group">
              <label for="fecha-input">Fecha de estreno</label>
              <input id="fecha-input" class="form-control" v-model="peliculaCreate.fecha" type="date" required>
              <div class="invalid-feedback">La fecha de estreno es requerida</div>
            </div>
          </form>
        </div>
      </div>
    </div>

    <b-modal id="modal-prevent-closing" ref="modal-prevent-closing" title="Actualizar película" @show="resetModal"
      @hidden="resetModal" @ok="handleOk">
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <b-form-group label="Name" label-for="name-input" invalid-feedback="Name is required" :state="nameUpdateState">
          <b-form-input id="name-input" v-model="peliculaUpdateName" :state="nameUpdateState" required></b-form-input>
        </b-form-group>
        <b-form-group label="Director" label-for="director-input" invalid-feedback="Director is required"
          :state="directorUpdateState">
          <b-form-input id="director-input" v-model="peliculaUpdateDirector" :state="directorUpdateState"
            required></b-form-input>
        </b-form-group>
        <b-form-group label="Género" label-for="genre-input" invalid-feedback="Género is required"
          :state="genreUpdateState">
          <b-form-select id="genre-input" v-model="peliculaUpdateGenre" :state="genreUpdateState" required>
            <option v-for="genre in genres" :key="genre.idGenre" :value="genre.idGenre">{{ genre.name }}
            </option>
          </b-form-select>
        </b-form-group>
        <b-form-group label="Duración" label-for="duration-input" invalid-feedback="Duración is required"
          :state="durationUpdateState">
          <b-form-input id="duration-input" v-model="peliculaUpdateDuration" :state="durationUpdateState"
            required></b-form-input>
        </b-form-group>
        <b-form-group label="Fechade estreno" label-for="fecha-input" invalid-feedback="Date is required"
          :state="fechaUpdateState">
          <b-form-input id="fecha-input" v-model="peliculaUpdateFecha" :state="fechaUpdateState" type="date"
            required></b-form-input>
        </b-form-group>
      </form>
    </b-modal>
  </div>
</template>

<style>
body {
  overflow-x: hidden;
}

.row {
  margin-right: 220px;
  padding-right: 200px;
}

.peliculas-container {
  padding-left: 20px;
  margin-top: 10px;
}

.pelicula-card {
  margin-bottom: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
}

.card-img-top {
  height: 200px;
  object-fit: cover;
}

.card-body {
  padding: 15px;
}

.form-container {
  position: fixed;
  top: 20px;
  /* Ajusta este valor según sea necesario */
  right: 20px;
  /* Ajusta este valor según sea necesario */
  padding: 20px;
  background-color: #f8f9fa;
  width: 400px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 20px;
}

label {
  font-weight: bold;
}

.input-field {
  width: 100%;
  padding: 10px;
  border: 1px solid #ced4da;
  border-radius: 5px;
}

.invalid-feedback {
  color: #dc3545;
  display: none;
}

.invalid-feedback.active {
  display: block;
}

.drop-zone {
  background-color: greenyellow;
  width: 500px;
  /* Ajusta el ancho según sea necesario */
  height: 100px;
  /* Ajusta la altura según sea necesario */
  top: 20px;
  border-radius: 5%;
  padding: 10px;
  margin-top: 50px;
  margin-bottom: 50px;
}

.drag-el {
  background-color: #fff;
  margin-bottom: 10px;
  padding: 5px;
}
</style>

<script>

import movieService from '@/services/Movie'

export default {
  data() {
    return {
      genres: [
      ],
      peliculas: [
      ],
      peliculaCreate: {
        name: '',
        director: '',
        genre: '',
        duration: '',
        fecha: ''
      },
      nameCreateState: null,
      directorCreateState: null,
      genreCreateState: null,
      durationCreateState: null,
      fechaCreateState: null,
      peliculaUpdate: {
        name: '',
        director: '',
        genre: '',
        duration: '',
        fecha: ''
      },
      nameUpdateState: null,
      directorUpdateState: null,
      genreUpdateState: null,
      durationUpdateState: null,
      fechaUpdateState: null,
      peliculaUpdateName: '',
      peliculaUpdateDirector: '',
      peliculaUpdateGenre: '',
      peliculaUpdateDuration: '',
      peliculaUpdateFecha: '',

      //scroll
      showElement: true,
      lastScrollPosition: 0
    }
  },
  mounted() {
    window.addEventListener("scroll", this.onScroll);
    this.getPeliculas();
    this.getGenres();
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.onScroll);
  },
  methods: {
    checkFormValidity() {
      const valid = this.$refs.form.checkValidity()
      return valid
    },
    resetModal() {
      this.peliculaCreate = {
        name: '',
        director: '',
        genre: '',
        duration: '',
        fecha: ''
      }
      this.nameCreateState = null
      this.directorCreateState = null
      this.genreCreateState = null
      this.durationCreateState = null
      this.fechaCreateState = null
      this.peliculaUpdate = {
        name: '',
        director: '',
        genre: '',
        duration: '',
        fecha: ''
      }
      this.nameUpdateState = null
      this.directorUpdateState = null
      this.genreUpdateState = null
      this.durationUpdateState = null
      this.fechaUpdateState = null
    },
    handleOk(bvModalEvent) {
      // Prevent modal from closing
      bvModalEvent.preventDefault()
      // Trigger submit handler
      this.handleSubmit()
    },
    createMovie() {
      if (!this.checkFormValidity()) {
        return
      }
      this.$nextTick(() => {
        this.postPelicula()
        this.$bvModal.hide('modal-create')
        alert('Película guardada correctamente')
      })
    },
    createMovieFromDrag() {
      console.log('Creando película')
      if (!this.checkFormValidity()) {
        alert('Campos vacíos')
        return
      }
      this.postPelicula()
      alert('Película creada correctamente')
      //Limpiar campos
      this.peliculaCreate.name = ''
      this.peliculaCreate.director = ''
      this.peliculaCreate.genre = ''
      this.peliculaCreate.duration = ''
      this.peliculaCreate.fecha = ''
    },
    handleSubmit() {
      // Exit when the form isn't valid
      if (!this.checkFormValidity()) {
        return
      }
      // Push the name to submitted names
      this.submittedNames.push(this.name)
      // Hide the modal manually
      this.$nextTick(() => {
        this.$bvModal.hide('modal-prevent-closing')
      })
      this.putMovie()
    },
    async getPeliculas() {
      console.log('Obteniendo películas')
      try {
        const response = await movieService.getMovies()
        console.log(response.data)
        this.peliculas = response.data
      } catch (error) {
        console.error(error)
      }
    },
    async getPelicula(idMovie) {
      try {
        console.log('Obteniendo película')
        console.log(idMovie)
        const response = await movieService.getMovie(idMovie)
        console.log(response.data)
        this.peliculaUpdate = response.data
        this.peliculaUpdateName = response.data.name
        this.peliculaUpdateDirector = response.data.director
        this.peliculaUpdateGenre = response.data.genre.idGenre
        this.peliculaUpdateDuration = response.data.duration
        this.peliculaUpdateFecha = response.data.releaseDate ? response.data.releaseDate.split('T')[0] : ''
      } catch (error) {
        console.error(error)
      }
    },
    async postPelicula() {
      try {
        console.log('Creando película')
        console.log(this.peliculaCreate)
        const movie = {
          name: this.peliculaCreate.name,
          director: this.peliculaCreate.director,
          genre: {
            idGenre: this.peliculaCreate.genre
          },
          duration: this.peliculaCreate.duration,
          releaseDate: this.peliculaCreate.fecha
        }
        const response = await movieService.postMovie(movie)
        console.log(response.data)
        this.getPeliculas()
      } catch (error) {
        console.error(error)
      }
    },
    async deleteMovie(idMovie) {
      try {
        console.log('Eliminando película')
        console.log(idMovie)
        const response = await movieService.deleteMovie(idMovie)
        console.log(response.data)
        alert('Película eliminada correctamente')
        this.getPeliculas()
      } catch (error) {
        console.error(error)
      }
    },
    async getGenres() {
      try {
        console.log('Obteniendo géneros')
        const response = await movieService.getGenres()
        console.log(response.data)
        this.genres = response.data
      } catch (error) {
        console.error(error)
      }
    },
    async putMovie() {
      try {
        let idMovie = this.peliculaUpdate.idMovie
        console.log('Actualizando película')
        console.log(this.peliculaUpdate)
        const movie = {
          name: this.peliculaUpdateName,
          director: this.peliculaUpdateDirector,
          genre: {
            idGenre: this.peliculaUpdateGenre
          },
          duration: this.peliculaUpdateDuration,
          releaseDate: this.peliculaUpdateFecha
        }
        const response = await movieService.putMovie(idMovie, movie)
        console.log(response.data)
        alert('Película actualizada correctamente')
        this.getPeliculas()
      } catch (error) {
        console.error(error)
      }
    },
    onDrop(event) {
      console.log('Soltado');
      console.log(event);
      this.createMovieFromDrag();

    },

    startDrag(event) {
      console.log('Drag started');
      console.log(event);
    },

    //scroll
    onScroll() {
      // Obtiene la posición actual del scroll
      const currentScrollPosition = window.pageYOffset || document.documentElement.scrollTop;
      console.log(currentScrollPosition);

      // La función abs para tener valores absolutos y se delimita con un offset o bien llamado 
      // margen para que el valor de la posición sea después de desplazarce y no desde que uno se desplaza
      if (Math.abs(currentScrollPosition - this.lastScrollPosition) < 60) {
        return;
      }
      // aqui determinamos si la posición es mayor a la posición anterior. Entonces, si lo es, mostramos el elemento.
      this.showElement = currentScrollPosition < this.lastScrollPosition;
      //  
      this.lastScrollPosition = currentScrollPosition;
    },

  }
}
</script>
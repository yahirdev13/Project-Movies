<template>
  <div id="about" class="cinema-bg">
    <header class="text-light text-center py-5">
      <div class="container">
        <div class="row">
          <div class="col-md-8 mx-auto">
            <h1 class="display-3 fancy-font" style="font-size: 6em;">¡Bienvenido al nuevo Netflix!</h1>
            <p class="lead">Explora nuestro catálogo de películas</p>
            <router-link to="/movies" class="btn btn-primary btn-lg mt-4">Ver catálogo Completo</router-link>
          </div>
        </div>
      </div>
    </header>

    <footer class="text-light text-center py-3">
      <p>&copy; Todos los derechos reservados.</p>
    </footer>
  </div>
</template>

<script>
export default {};
</script>

<style>
#about {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  align-items: center;
  justify-content: space-between;
  background-color: #000;
  background-image: url('../assets/netflix.png');
  background-size: cover;
  background-position: center;
}

.cinema-bg {
  background-color: rgba(0, 0, 0, 0.9);
  /* Añade un fondo oscuro semi-transparente */
}

.btn-primary {
  background-color: #ec1c24;
  /* Color primario de Cinépolis */
  border-color: #ec1c24;
  /* Color del borde del botón */
}

.btn-primary:hover {
  background-color: #c41218;
  /* Cambio de color al pasar el mouse */
  border-color: #c41218;
}

.text-light {
  color: #fff;
  /* Texto claro */
}
</style>
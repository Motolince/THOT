<template>
  <div class="">
    <div
      class="row align-items-center justify-content-center justify-content-md-between"
    >
      <div class="col-6 my-2 col-md-3 text-center">
        <img src="~/static/logo_ananke.png" class="w-100" alt="ANANKE" />
        <!-- <h1 class="display-4 text-center">ANANKE</h1> -->
      </div>
      <div class="col-12 col-md-9">
        <b-nav tabs justified>
          <b-nav-item to="/" exact exact-active-class="active">
            <b-icon icon="controller"></b-icon>
            CONTROLES
          </b-nav-item>
          <b-nav-item to="/movements-log" exact exact-active-class="active">
            <b-icon icon="file-earmark-spreadsheet"></b-icon>
            MOVIMIENTOS
          </b-nav-item>
          <b-nav-item to="/about" exact exact-active-class="active">
            <b-icon icon="question-circle"></b-icon>
            <span class="d-none d-md-inline-block">ACERCA DE</span>
          </b-nav-item>
        </b-nav>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Navigation"
};
</script>

<style>
.nav {
  white-space: nowrap;
}
.nav-link {
  color: #2d2d2d;
  border-bottom-color: #2d2d2d !important;
  font-weight: bold;
}
.nav-link:hover {
  color: #2d2d2d;
  background-color: #ddd;
  transition: all 0.5s;
}

.nuxt-link-active {
  background-color: #f59b1a !important;
  color: #2d2d2d !important;
  border-color: #2d2d2d !important;
  border-bottom-color: #f59b1a !important;
}
</style>

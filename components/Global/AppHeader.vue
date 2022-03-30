<template>
  <div>
    <b-navbar toggleable="lg" type="dark" class="navbar">
      <b-navbar-brand href="/" class="navbar__brand">Ultimate</b-navbar-brand>

      <b-navbar-nav right class="ml-auto">
        <b-nav-item href="/dataTable" class="navbar__item"
          >Data Table</b-nav-item
        >
      </b-navbar-nav>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: 'header',
}
</script>
<style>
.navbar {
  background-color: rgb(153, 152, 153);
}
.navbar__brand {
  font-size: 2.2rem;
}
.navbar__item {
  font-size: 1.8rem;
}
</style>

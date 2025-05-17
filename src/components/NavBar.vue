<template>

<nav class="navbar">
  <div class="container">
    <a class="navbar-brand">{{ title }}</a>
    <form class="d-flex" role="search">
      <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search"/>
      <button class="btn btn-outline-primary" type="submit">Search</button>
    </form>
  </div>
</nav>

</template>


<script setup>
defineProps({
  title: String,
})
</script>

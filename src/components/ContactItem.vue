<script setup>
defineProps({
  contact: Object,
});
defineEmits(['edit', 'delete']);
</script>

<template>
<div class="card mb-2">
  <div class="card-body">
    <h5 class="card-title">{{ contact.name }}</h5>
    <p class="card-text">Email: {{ contact.email }}</p>
    <p class="card-text">Téléphone: {{ contact.phone }}</p>
    <button class="btn btn-sm btn-info" @click="$emit('edit', contact)">Modifier</button>
    <button class="btn btn-sm btn-danger ms-2" @click="$emit('delete', contact)">Supprimer</button>
  </div>
</div>
</template>


<style scoped>
</style>

<template>
  <div class="modal-backdrop fade show"></div>
  <div class="modal show d-block" tabindex="-1" role="dialog">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">Confirmer la suppression</h5>
          <button type="button" class="btn-close" aria-label="Close" @click="$emit('cancel')"></button>
        </div>
        <div class="modal-body">
          <p>Êtes-vous sûr de vouloir supprimer le contact "<strong>{{ contact.name }}</strong>" ?</p>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" @click="$emit('cancel')">Annuler</button>
          <button type="button" class="btn btn-danger" @click="$emit('confirm')">Supprimer</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  contact: Object,
});
</script>

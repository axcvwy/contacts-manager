<template>
  <div class="modal-backdrop fade show"></div>
  <div class="modal show d-block" tabindex="-1" role="dialog">
    <div class="modal-dialog">
      <form @submit.prevent="updateContact" class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">Edit Contact</h5>
          <button type="button" class="btn-close" @click="$emit('close')"></button>
        </div>
        <div class="modal-body">
          <div class="mb-3">
            <label class="form-label">Nom</label>
            <input v-model="localContact.name" type="text" class="form-control" />
          </div>

          <div class="mb-3">
            <label class="form-label">Email</label>
            <input v-model="localContact.email" type="text" class="form-control" />
          </div>

          <div class="mb-3">
            <label class="form-label">Téléphone</label>
            <input v-model="localContact.phone" type="text" class="form-control" />
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" @click="$emit('close')">Cancel</button>
          <button type="submit" class="btn btn-primary">Update</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";
import axios from "axios";
import { useToast } from "vue-toastification";

const props = defineProps({
  contact: Object,
});

const emit = defineEmits(["updated", "close"]);

const toast = useToast();

const localContact = ref({ ...props.contact });

watch(
  () => props.contact,
  (newVal) => {
    localContact.value = { ...newVal };
  }
);

const updateContact = async () => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  const phoneRegex = /^[0-9]+$/;

  if (!localContact.value.name) {
    toast.error("Le nom est requis.");
    return;
  }

  if (!localContact.value.email) {
    toast.error("L'email est requis.");
    return;
  }

  if (!emailRegex.test(localContact.value.email)) {
    toast.error("Le format de l'email est invalide.");
    return;
  }

  if (!localContact.value.phone) {
    toast.error("Le téléphone est requis.");
    return;
  }

  if (!phoneRegex.test(localContact.value.phone)) {
    toast.error("Le téléphone doit contenir uniquement des chiffres.");
    return;
  }
  try {
    const response = await axios.put(
      `http://localhost:3000/contacts/${localContact.value.id}`,
      localContact.value
    );

    if (response.status === 200) {
      toast.success("Contact mis à jour avec succès.");
      emit("updated");
      emit("close");
    }
  } catch (err) {
    console.error(err);
    toast.error("Erreur lors de la mise à jour du contact.");
  }
};
</script>

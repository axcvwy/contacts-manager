<template>
  <p class="d-inline-flex gap-1">
    <button
      class="btn btn-primary"
      type="button"
      data-bs-toggle="collapse"
      data-bs-target="#collapseExample"
      aria-expanded="false"
      aria-controls="collapseExample"
    >
      Créer un nouveau contact
    </button>
  </p>
  <div class="collapse" id="collapseExample">
    <div class="card card-body">
      <form @submit.prevent="saveContact">
        <div class="mb-3">
          <label class="form-label">Nom</label>
          <input v-model="contact.name" type="text" class="form-control" />
        </div>

        <div class="mb-3">
          <label class="form-label">Email</label>
          <input v-model="contact.email" type="text" class="form-control" />
        </div>

        <div class="mb-3">
          <label class="form-label">Téléphone</label>
          <input v-model="contact.phone" type="text" class="form-control" />
        </div>

        <button type="submit" class="btn btn-primary">Créer</button>
      </form>
    </div>
  </div>
</template>



<script setup>
import { ref } from "vue";
import axios from "axios";
import { useToast } from "vue-toastification";

const emit = defineEmits(["added"]);

const contact = ref({
  name: "",
  email: "",
  phone: "",
});

const toast = useToast();

const saveContact = async () => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  const phoneRegex = /^[0-9]+$/;

  if (!contact.value.name) {
    toast.error("Le nom est requis.");
    return;
  }

  if (!contact.value.email) {
    toast.error("L'email est requis.");
    return;
  }

  if (!emailRegex.test(contact.value.email)) {
    toast.error("Le format de l'email est invalide.");
    return;
  }

  if (!contact.value.phone) {
    toast.error("Le téléphone est requis.");
    return;
  }

  if (!phoneRegex.test(contact.value.phone)) {
    toast.error("Le téléphone doit contenir uniquement des chiffres.");
    return;
  }

  try {
    const response = await axios.post(
      "http://localhost:3000/contacts",
      contact.value
    );
    if (response.status === 201) {
      toast.success("Contact ajouté avec succès");
      emit("added");
      contact.value = { name: "", email: "", phone: "" };
    }
  } catch (err) {
    console.error(err);
    toast.error("Erreur lors de l'ajout du contact.");
  }
};
</script>

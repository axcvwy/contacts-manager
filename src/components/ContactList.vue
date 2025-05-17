<template>
  <div v-if="loading">
    <div class="alert alert-primary" role="alert">Loading contacts...</div>
  </div>
  <div v-else>
    <div v-if="error">
      <div class="alert alert-danger" role="alert">{{ error }}</div>
    </div>
    <div v-else-if="contacts.length === 0">
      <div class="alert alert-warning" role="alert">Aucun contact trouvé</div>
    </div>
    <ul v-else>
      <ContactItem
        v-for="contact in contacts"
        :key="contact.id"
        :contact="contact"
        @edit="openEditModal"
        @delete="openDeleteModal"
      />
      <EditContactModal
        v-if="showModal"
        :contact="selectedContact"
        @updated="getContacts"
        @close="showModal = false"
      />
      <ConfirmDeleteModal
        v-if="showDeleteModal"
        :contact="contactToDelete"
        @cancel="cancelDelete"
        @confirm="confirmDelete"
      />
    </ul>
  </div>
</template>



<script setup>
import { ref, onMounted, watch } from "vue";
import axios from "axios";

import ContactItem from "./ContactItem.vue";
import EditContactModal from "./EditContactModal.vue";
import ConfirmDeleteModal from "./ConfirmDeleteModal.vue"; // import the modal

const props = defineProps({
  refresh: Boolean,
});

const showModal = ref(false);
const selectedContact = ref(null);

const showDeleteModal = ref(false);
const contactToDelete = ref(null);

const openEditModal = (contact) => {
  selectedContact.value = { ...contact };
  showModal.value = true;
};

const contacts = ref([]);
const loading = ref(true);
const error = ref(null);


const getContacts = async () => {
  try {
    const response = await axios.get("http://localhost:3000/contacts");
    contacts.value = response.data;
  } catch (err) {
    error.value = "Failed to load contacts.";
    console.log(err);
  } finally {
    loading.value = false;
  }
};

const openDeleteModal = (contact) => {
  contactToDelete.value = contact;
  showDeleteModal.value = true;
};

const cancelDelete = () => {
  showDeleteModal.value = false;
  contactToDelete.value = null;
};

const confirmDelete = async () => {
  try {
    await axios.delete(`http://localhost:3000/contacts/${contactToDelete.value.id}`);
    contacts.value = contacts.value.filter((c) => c.id !== contactToDelete.value.id);
    showDeleteModal.value = false;
    contactToDelete.value = null;
  } catch (err) {
    console.error(err);
    alert("Erreur lors de la suppression du contact.");
  }
};

onMounted(() => {
  getContacts();
});

watch(
  () => props.refresh,
  () => {
    getContacts();
  }
);
</script>


<style scoped>
ul {
  margin-left: 0;
  padding-left: 0;
  list-style: none;
}
</style>

<template>
  <div class="container">
  <NavBar title="Contacts"/>

  <div class="container mt-4">
    <ContactForm @added="reloadContacts" />
    <hr />
    <ContactList :refresh="isRefreshing" />
  </div>

  </div>
</template>


<script setup>
import NavBar from "@/components/NavBar.vue";
import ContactForm from "@/components/ContactForm.vue";
import ContactList from "@/components/ContactList.vue";
import { ref } from "vue";

const isRefreshing = ref(false);

const reloadContacts = () => {
  isRefreshing.value = !isRefreshing.value;
};
</script>

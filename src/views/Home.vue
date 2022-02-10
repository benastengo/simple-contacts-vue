<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      contacts: [],
      newContacts: {},
    };
  },
  created: function () {
    this.contactsIndex();
  },
  methods: {
    contactsIndex: function () {
      axios.get("http://localhost:3000/contacts").then((response) => {
        console.log("sanity", response.data);
        this.contacts = response.data;
      });
    },
    createContacts: function () {
      axios
        .post("http://localhost:3000/contacts", this.newContacts)
        .then((response) => {
          console.log("Contact Create", response);
          this.contacts.push(response.data);
          this.newContacts = {};
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>New Contact</h1>
    <div>
      First Name
      <input type="text" v-model="newContacts.first_name" />
      Last Name
      <input type="text" v-model="newContacts.last_name" />
      Email
      <input type="text" v-model="newContacts.email" />
      Phone Number
      <input type="number" v-model="newContacts.phone_number" />
      Image
      <input type="text" v-model="newContacts.image" />
      <button v-on:click="createContacts()">Create A New Contact</button>
    </div>
    <h1>All Contacts</h1>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <h2>{{ contact.first_name }}</h2>
    </div>
  </div>
</template>

<style></style>

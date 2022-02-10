<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      contacts: [],
      newContacts: {},
      currentContact: {},
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
    showContact: function (contact) {
      this.currentContact = contact;
      document.querySelector("#Contact-details").showModal();
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
      <button v-on:click="showContact(contact)">Show Contact Info</button>
    </div>
    <dialog id="Contact-details">
      <form method="dialog">
        <h1>Contact Info</h1>
        <p>First Name: {{ currentContact.first_name }}</p>
        <p>Last Name: {{ currentContact.last_name }}</p>
        <p>Email: {{ currentContact.email }}</p>
        <p>Phone Number: {{ currentContact.phone_number }}</p>
        <p>Image: {{ currentContact.image }}</p>
        <button>CLOSE</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>

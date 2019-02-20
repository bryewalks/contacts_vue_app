<template>
  <div class="home">
    <h1>New Contact</h1>
    <div>
      <div>
        First Name: <input v-model="newContactFirstName">
      </div>
      <div>
        Middle Name: <input v-model="newContactMiddleName">        
      </div>
      <div>
        Last Name: <input v-model="newContactLastName">
      </div>
      <div>
        Email: <input v-model="newContactEmail">
      </div>
      <div>
        Phone Number: <input v-model="newContactPhoneNumber">
      </div>
      <div>
        Bio: <input v-model="newContactBio">
      </div>
      <button v-on:click="createContact()">Add Contact</button>
    </div>

    <h1>All Contacts</h1>
    <div v-for="contact in contacts">
      <h2>{{ contact.first_name + " " + contact.last_name}}</h2>
      <div>   
        <button v-on:click="showContact(contact)">Contact Info</button>
      </div>
      <div v-if="contact === currentContact"> 
        <p>First Name: {{ contact.first_name }}</p>
        <div v-if="contact.middle_name">
          <p>Middle Name: {{ contact.middle_name }}</p>
        </div>
        <p>Last Name: {{ contact.last_name }}</p>
        <p>Email: {{ contact.email }}</p>
        <p>Phone Number: {{ contact.phone_number }}</p>
        <p>Bio: {{ contact.bio }}</p>
        <div>
          <div>
            First Name: <input v-model="contact.first_name">
          </div>
          <div>
            Middle Name: <input v-model="contact.middle_name">        
          </div>
          <div>
            Last Name: <input v-model="contact.last_name">
          </div>
          <div>
            Email: <input v-model="contact.email">
          </div>
          <div>
            Phone Number: <input v-model="contact.phone_number">
          </div>
          <div>
            Bio: <input v-model="contact.bio">
          </div>
          <button v-on:click="updateContact(contact)">Edit Contact</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import axios from "axios";
var axios = require("axios");

export default {
  data: function() {
    return {
      contacts: [],
      newContactFirstName: "",
      newContactMiddleName: "",
      newContactLastName: "",
      newContactEmail: "",
      newContactPhoneNumber: "",
      newContactBio: "",
      currentContact: {}
      // newContactFullName: ""
    };
  },
  created: function() {
    axios.get("/api/contacts")
      .then(response => {
      this.contacts = response.data;
    });
  },
  methods: {
    showContact: function(inputContact) {
      if (this.currentContact === inputContact) {
        this.currentContact = {};
      } else {
        this.currentContact = inputContact;
      }
    },
    createContact: function() {
      var params = {
                    first_name: this.newContactFirstName,
                    middle_name: this.newContactMiddleName,
                    last_name: this.newContactLastName,
                    email: this.newContactEmail,
                    phone_number: this.newContactPhoneNumber,
                    bio: this.newContactBio
                    };
      axios.post("/api/contacts", params)
        .then(response => {
          console.log("Success", response.data);
          this.contacts.push(response.data); 
        });
    },
    updateContact: function(inputContact) {
      var params = {
                    first_name: inputContact.first_name,
                    middle_name: inputContact.middle_name,
                    last_name: inputContact.last_name,
                    email: inputContact.email,
                    phone_number: inputContact.phone_number,
                    bio: inputContact.bio
                    };
      axios.patch("/api/contacts/" + inputContact.id)
        .then(response => {
          console.log("Success", response.data);
        });
    }
  }
};
</script>

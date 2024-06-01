<script setup>
import { ref } from "vue"
import allContacts from "./contacts.json"

const initialContactsNum = 5
// inicializar la lista displayedContacts con los primeros contactos
const displayedContacts = ref(allContacts.slice(0, initialContactsNum))
// quitar los primeros contactos de la lista nonDisplayedContacts
let nonDisplayedContacts = allContacts.filter(
  (contact, index) => index > initialContactsNum
)

const extractRandomContact = () => {
  const remainingContactsNum = nonDisplayedContacts.length
  if (remainingContactsNum < 1) {
    alert("No more contacts to display")
    return
  }

  // coger un contacto aleatorio de la lista nonDisplayedContacts
  const randomIndex = Math.floor(Math.random() * remainingContactsNum)
  const randomContact = nonDisplayedContacts[randomIndex]

  // quitar de la lista nonDisplayedContacts
  nonDisplayedContacts = nonDisplayedContacts.filter(
    (contact, index) => index !== randomIndex
  )

  // añadir a la lista displayedContacts
  displayedContacts.value.push(randomContact)
}

const restoreContact = (id) => {
  // añadir a la lista nonDisplayedContacts
  const contact = displayedContacts.value.find((contact) => contact.id === id)
  nonDisplayedContacts.push(contact)

  // quitar de la lista displayedContacts
  displayedContacts.value = displayedContacts.value.filter(
    (contact) => contact.id !== id
  )
}

const sortByPopularity = () => {
  displayedContacts.value.sort((a, b) => b.popularity - a.popularity)
}

const sortByName = () => {
  displayedContacts.value.sort((a, b) => a.name.localeCompare(b.name))
}
//
</script>

<template>
  <header>
    <h1>IronContacts</h1>
  </header>
  <main>
    <div class="actions-bar">
      <div class="actions-section">
        <button @click="extractRandomContact" class="plain-button">+</button>
      </div>
      <div class="actions-section">
        <span>Sort by:</span>
        <button @click="sortByPopularity" class="plain-button">
          popularity
        </button>
        <button @click="sortByName" class="plain-button">name</button>
      </div>
    </div>
    <table class="contacts-table" v-if="displayedContacts.length > 0">
      <thead>
        <tr>
          <th>Picture</th>
          <th>Name</th>
          <th>Popularity</th>
          <th>Won Oscar</th>
          <th>Won Emmy</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in displayedContacts" :key="contact.id">
          <td class="profile-picture-cell">
            <img
              :src="contact.pictureUrl"
              :alt="`${contact.name}'s profile picture`"
              class="profile-picture"
            />
          </td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity.toFixed(2) }}</td>
          <td>{{ contact.wonOscar ? "🏆" : "" }}</td>
          <td>{{ contact.wonEmmy ? "🌟" : "" }}</td>
          <td>
            <button @click="restoreContact(contact.id)" class="bordered-button">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>No contacts to display</p>
  </main>
</template>

<style>
:root {
  --primary-color: #7a61fb;
  --primary-color-2: #e8e4f7;
  --primary-color-3: #d3cde9;
  --text-color: #647279;
  --background-color: #dbe4e7;
  --white: #f1f1f1;
  --border-radius-small: 6px;
  --border-radius-big: 12px;
  --shadow: 0 0 30px rgba(0, 0, 0, 0.1);
}
body {
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
  line-height: 1.5;
  margin: 0;
  padding: 2rem 4rem;
  background-color: var(--background-color);
  color: var(--text-color);
}

h1 {
  color: var(--primary-color);
  font-weight: 400;
  font-size: 2rem;
}

.actions-bar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.actions-section {
  display: flex;
  gap: 0.5rem;
  align-items: center;
}

button {
  padding: 0.5rem 0.7rem;
  border: none;
  border-radius: var(--border-radius-small);
  cursor: pointer;
  font-weight: 500;
  color: var(--text-color);
  background-color: var(--white);
  transition: background-color 100ms;
}
button:hover {
  background-color: var(--primary-color-2);
}
button:active {
  background-color: var(--primary-color-3);
}

.plain-button {
  border: none;
  box-shadow: var(--shadow);
}

.bordered-button {
  border: 1px solid var(--text-color);
}

.contacts-table {
  width: 100%;
  border-spacing: 0 0.5rem;
}

.contacts-table thead {
  position: sticky;
  top: 0;
}

.contacts-table th {
  text-align: start;
  font-size: 0.7rem;
  text-transform: uppercase;
}

.contacts-table th,
.contacts-table td {
  padding: 0.5rem 1rem;
}

.contacts-table tbody tr {
  border-radius: var(--border-radius-big);
  box-shadow: var(--shadow);
}

.contacts-table td {
  background-color: var(--white);
}

.contacts-table tr td:first-child {
  border-top-left-radius: var(--border-radius-big);
  border-bottom-left-radius: var(--border-radius-big);
}

.contacts-table tr td:last-child {
  border-top-right-radius: var(--border-radius-big);
  border-bottom-right-radius: var(--border-radius-big);
}

.profile-picture-cell {
  display: grid;
}

.profile-picture {
  width: 50px;
  height: 50px;
  object-fit: cover;
  border-radius: 50%;
  object-position: center -10px;
  border: 1px solid var(--text-color);
}
</style>

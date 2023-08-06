<script setup>
import contacts from "@/contacts.json"
import { ref, onMounted } from "vue"

const contactsAdded = ref([])

const initialItems = async () => {
  const selected = []
  const items = contacts
  const numItems = items.length

  while (selected.length < 5) {
    const randomIndex = Math.floor(Math.random() * numItems)
    const selectedItem = items[randomIndex]

    if (!selected.includes(selectedItem)) {
      selected.push(selectedItem)
    }
  }

  contactsAdded.value = selected
}

const addItem = () => {
  const items = contacts
  const numItems = items.length
  let newItem = null

  if (contactsAdded.value.length === numItems) {
    return
  }

  while (!newItem || contactsAdded.value.includes(newItem)) {
    const randomIndex = Math.floor(Math.random() * numItems)
    newItem = items[randomIndex]
  }

  contactsAdded.value.push(newItem)
}

const sortByName = () => {
  contactsAdded.value.sort((a, b) => a.name.localeCompare(b.name))
}

const sortByPopularity = () => {
  contactsAdded.value.sort((a, b) => b.popularity - a.popularity)
}

const deleteItem = (id) => {
  contactsAdded.value = contactsAdded.value.filter((item) => item.id !== id)
}

onMounted(() => {
  initialItems()
})
</script>

<template>
  <h1 class="m-10 text-center text-4xl font-bold">IronContacts</h1>
  <section class="grid place-items-center space-y-4 border-green-300">
    <div class="flex gap-x-4">
      <button
        @click="addItem"
        class="rounded-full border-2 border-green-500 px-3 py-0.5 text-sm font-bold uppercase hover:bg-green-500 active:border-green-600 active:bg-green-600"
      >
        add
      </button>
      <button
        @click="sortByName"
        class="rounded-full border-2 border-green-500 px-3 py-0.5 text-sm font-bold uppercase hover:bg-green-500 active:border-green-600 active:bg-green-600"
      >
        Sort by name
      </button>
      <button
        @click="sortByPopularity"
        class="rounded-full border-2 border-green-500 px-3 py-0.5 text-sm font-bold uppercase hover:bg-green-500 active:border-green-600 active:bg-green-600"
      >
        Sort by popularity
      </button>
    </div>

    <div class="relative overflow-x-auto">
      <table
        class="w-full text-left text-sm text-green-500 dark:text-green-400"
      >
        <thead class="bg-green-700 text-xs uppercase text-green-400">
          <tr>
            <th class="px-6 py-3">Photo</th>
            <th class="px-6 py-3">Name</th>
            <th class="px-6 py-3">Popularity</th>
            <th class="px-6 py-3">Won an Oscar</th>
            <th class="px-6 py-3">Won an Emmy</th>
            <th class="px-6 py-3">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="border-b border-green-700 bg-green-900"
            v-for="contact in contactsAdded"
            :key="contact.id"
          >
            <td class="px-6 py-4">
              <img class="aspect-auto w-16" :src="contact.pictureUrl" alt="" />
            </td>
            <td class="px-6 py-4">
              <p>{{ contact.name }}</p>
            </td>
            <td class="px-6 py-4">
              <p>{{ contact.popularity }}</p>
            </td>
            <td class="px-6 py-4 text-center">
              <p v-if="contact.wonOscar">🏆</p>
            </td>
            <td class="px-6 py-4 text-center">
              <p v-if="contact.wonEmmy">🏆</p>
            </td>
            <td class="px-6 py-4">
              <button
                class="rounded-full border-2 border-red-500 bg-red-500 px-3 py-0.5 text-sm font-bold uppercase text-white hover:border-red-600 hover:bg-red-600 active:border-red-700 active:bg-red-700"
                @click="deleteItem(contact.id)"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>

<style scoped></style>

<script setup>
import { ref, onMounted } from 'vue'
import BookItem from './components/BookItem.vue'

const books = ref([])
const newTitle = ref('')
const newAuthor = ref('')

function addBook() {
  if (newTitle.value.trim() === '' || newAuthor.value.trim() === '') {
    return
  }

  books.value.push({
    id: Date.now(),
    title: newTitle.value,
    author: newAuthor.value,
    read: false
  })

  newTitle.value = ''
  newAuthor.value = ''

  saveBooks()
}

function deleteBook(id) {
  books.value = books.value.filter(book => book.id !== id)
  saveBooks()
}

function updateBook(updatedBook) {
  const index = books.value.findIndex(book => book.id === updatedBook.id)

  if (index !== -1) {
    books.value[index] = updatedBook
  }

  saveBooks()
}

function saveBooks() {
  localStorage.setItem('books', JSON.stringify(books.value))
}

onMounted(() => {
  const savedBooks = localStorage.getItem('books')

  if (savedBooks) {
    books.value = JSON.parse(savedBooks)
  }
})
</script>

<template>
  <div class="container">
    <h2> My Wattpad Reading List</h2>

   
    <div class="add-book">
      <input
        v-model="newTitle"
        type="text"
        placeholder="Book title"
        @keyup.enter="addBook"
      />
      <input
        v-model="newAuthor"
        type="text"
        placeholder="Author name"
        @keyup.enter="addBook"
      />
      <button @click="addBook">Add Book</button>
    </div>

   
    <div class="book-list">
      <BookItem
        v-for="book in books"
        :key="book.id"
        :book="book"
        @delete="deleteBook"
        @update="updateBook"
      />
    </div>
  </div>
</template>

<style>
.container {
  width: 550px;
  margin: 50px auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
}

.add-book {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.add-book input {
  flex: 1;
  padding: 10px;
}

button {
  padding: 10px 15px;
  cursor: pointer;
}

.book-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
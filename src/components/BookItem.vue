<script setup>
import { ref } from 'vue'

const props = defineProps({
  book: Object
})

const emit = defineEmits([
  'delete',
  'update'
])

const isEditing = ref(false)
const editedTitle = ref(props.book.title)
const editedAuthor = ref(props.book.author)

function updateBook() {
  if (editedTitle.value.trim() === '' || editedAuthor.value.trim() === '') {
    return
  }

  emit('update', {
    ...props.book,
    title: editedTitle.value,
    author: editedAuthor.value
  })

  isEditing.value = false
}

function toggleRead() {
  emit('update', {
    ...props.book,
    read: !props.book.read
  })
}

function deleteBook() {
  emit('delete', props.book.id)
}

function startEditing() {
  editedTitle.value = props.book.title
  editedAuthor.value = props.book.author
  isEditing.value = true
}
</script>

<template>
  <div class="book-item">
 
    <div v-if="!isEditing" class="view-mode">
      <input
        type="checkbox"
        :checked="book.read"
        @change="toggleRead"
      />
      
      <div class="info">
        <span class="title" :class="{ completed: book.read }">
          "{{ book.title }}"
        </span>
        <span class="author">by {{ book.author }}</span>
      </div>

      <button @click="startEditing">Edit</button>
      <button @click="deleteBook">Delete</button>
    </div>

 
    <div v-else class="edit-mode">
      <input
        v-model="editedTitle"
        type="text"
        placeholder="Book title"
        @keyup.enter="updateBook"
      />
      <input
        v-model="editedAuthor"
        type="text"
        placeholder="Author"
        @keyup.enter="updateBook"
      />
      <button @click="updateBook">Save</button>
      <button @click="isEditing = false">Cancel</button>
    </div>
  </div>
</template>

<style scoped>
.book-item {
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  background-color: #fafafa;
}

.view-mode {
  display: flex;
  align-items: center;
  gap: 10px;
}
.edit-mode {
  display: flex;
  align-items: center;
  gap: 10px;
}


.info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.title {
  font-weight: bold;
}

.author {
  font-size: 0.85rem;
  color: #666;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

button {
  padding: 6px 12px;
  cursor: pointer;
}

input[type="text"] {
  padding: 6px;
  flex: 1;
}
</style>
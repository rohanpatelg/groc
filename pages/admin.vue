<template>
  <div class="container mx-auto p-4">
    <h1 class="text-2xl font-bold mb-6">Admin</h1>
    
    <div class="max-w-2xl mx-auto">
      <!-- Edit Button -->
      <button 
        v-if="!isEditing" 
        @click="toggleEdit" 
        class="mb-4 px-4 py-2 bg-blue-500 text-white rounded"
      >
        Edit
      </button>

      <!-- Grocery List -->
      <div class="space-y-4">
        <div v-for="(item, index) in groceryList" :key="index" class="flex items-center gap-4">
          <input 
            v-model="item.name" 
            :disabled="!isEditing"
            class="border p-2 w-48"
            placeholder="Item"
          />
          <input 
            v-model="item.price" 
            :disabled="!isEditing"
            class="border p-2 w-24"
            type="number"
            step="0.01"
            placeholder="Rs/Kg"
          />
          <!-- Add Delete Button -->
          <button 
            v-if="isEditing"
            @click="deleteItem(index)" 
            class="px-3 py-2 bg-red-500 text-white rounded"
          >
            Delete
          </button>
        </div>
      </div>

      <!-- Action Buttons -->
      <div v-if="isEditing" class="mt-6 space-x-4">
        <button 
          @click="addNewItem" 
          class="px-4 py-2 bg-green-500 text-white rounded"
        >
          Add
        </button>
        <button 
          @click="saveList" 
          class="px-4 py-2 bg-blue-500 text-white rounded"
        >
          Save
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
const isEditing = ref(false)
const groceryList = ref([
  { name: 'Potato', price: 1.22 },
  { name: 'Tomato', price: 0.09 },
  { name: 'Onion', price: 9.0 },
  { name: 'Lauki', price: 2.2 }
])

// Load data from localStorage on component mount
onMounted(() => {
  const savedList = localStorage.getItem('groceryList')
  if (savedList) {
    groceryList.value = JSON.parse(savedList)
  }
})

const toggleEdit = () => {
  isEditing.value = true
}

const addNewItem = () => {
  groceryList.value.push({ name: '', price: '' })
}

const saveList = () => {
  localStorage.setItem('groceryList', JSON.stringify(groceryList.value))
  isEditing.value = false
}

// Add delete function
const deleteItem = (index) => {
  groceryList.value.splice(index, 1)
}
</script>

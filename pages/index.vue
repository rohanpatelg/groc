<template>
  <div class="container mx-auto p-4">
    <div class="max-w-2xl mx-auto">
      <!-- Clear Button -->
      <div class="flex justify-end mb-4">
        <button
          @click="clearWeights"
          class="px-4 py-2 bg-gray-500 text-white rounded"
        >
          Clear
        </button>
      </div>

      <!-- Headers -->
      <div class="flex gap-4 mb-4">
        <div class="w-48 font-bold">Item</div>
        <div class="w-24 font-bold">KG</div>
        <div class="w-32 font-bold">Value</div>
      </div>

      <!-- Grocery List -->
      <div class="space-y-4">
        <div
          v-for="(item, index) in groceryList"
          :key="index"
          class="flex items-center gap-4"
        >
          <div class="w-48 border p-2">{{ item.name }}</div>
          <input
            v-model="weights[index]"
            type="number"
            step="0.01"
            class="border p-2 w-24"
            placeholder="0.00"
            @input="calculateTotal"
          />
          <div class="w-32 border p-2">
            {{ calculateItemValue(index) }}
          </div>
        </div>
      </div>
      <div class="flex mt-8 justify-start items-center gap-4 mb-4">
        <!-- Amount of given given by the customer -->
        <!-- Total -->
        <div class="flex w-full justify-between items-center mt-6 pt-4 border-t">
          <div class="font-bold">Total</div>
          <div class="border p-2 min-w-[20px] max-w-48">{{ totalPrice }}</div>
        </div>
      </div>
      <div class="flex items-center justify-between mt-6 pt-4 border-t">
        <div>Money given by customer</div>
      <input
        v-model="givenAmount"
        type="number"
        step="0.01"
        class="border p-2 w-24 ml-8"
        placeholder="0.00"
        @input="calculateNetAmount"
        />
      </div>
      <div class="flex justify-between items-center mt-6 pt-4 border-t">
        <div class="mt-4">Net Amount (jo dena h tumko)</div>
        <div class="border p-2 px-4 w-fit">{{ netAmount }}</div>
      </div>

    </div>
  </div>
</template>

<script setup>
const groceryList = ref([]);
const weights = ref([]);
const totalPrice = ref(0);
const netAmount = ref(0);
// Load grocery list from localStorage
onMounted(() => {
  const savedList = localStorage.getItem("groceryList");
  if (savedList) {
    groceryList.value = JSON.parse(savedList);
    // Initialize weights array with empty values
    weights.value = new Array(groceryList.value.length).fill("");
  }
});
const givenAmount = ref(0);
// Calculate value for individual item
const calculateItemValue = (index) => {
  const itemPrice = groceryList.value[index].price;
  const itemWeight = parseFloat(weights.value[index]) || 0;
  return (itemPrice * itemWeight).toFixed(2);
};

// Calculate total price based on weights and price per kg
const calculateTotal = () => {
  totalPrice.value = weights.value.reduce((sum, weight, index) => {
    const itemPrice = groceryList.value[index].price;
    const itemWeight = parseFloat(weight) || 0;
    return sum + itemPrice * itemWeight;
  }, 0);

  // Format to 2 decimal places
  totalPrice.value = totalPrice.value.toFixed(2);
  calculateNetAmount();
};
const calculateNetAmount = () => {
  if (totalPrice.value > 0 && givenAmount.value > totalPrice.value) {
    netAmount.value = givenAmount.value - totalPrice.value;
  } else {
    netAmount.value = 0;
  }
};

// Clear all weights and reset total
const clearWeights = () => {
  weights.value = weights.value.map(() => "");
  totalPrice.value = 0;
  givenAmount.value = 0;
  netAmount.value = 0;
};
</script>

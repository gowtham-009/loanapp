<template>
  <div>
    <!-- Row 1 -->
    <input type="number" v-model.number="val1_1" placeholder="Enter first number">
    <input type="number" v-model.number="val2_1" placeholder="Enter second number">
    <input type="text" :value="val3_1" placeholder="Result" readonly>
  </div>

  <div>
    <!-- Row 2 -->
    <input type="number" v-model.number="val1_2" placeholder="Enter first number">
    <input type="number" v-model.number="val2_2" placeholder="Enter second number">
    <input type="text" :value="val3_2" placeholder="Result" readonly>
  </div>

  <div>
    <!-- Row 3 -->
    <input type="number" v-model.number="val1_3" placeholder="Enter first number">
    <input type="number" v-model.number="val2_3" placeholder="Enter second number">
    <input type="text" :value="val3_3" placeholder="Result" readonly>
  </div>

  <div>
    <strong>Total: </strong>
    <input type="text" :value="total" placeholder="Total Sum" readonly>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

// Row 1
const val1_1 = ref(2);
const val2_1 = ref(0);
const val3_1 = computed(() => (val1_1.value * val2_1.value).toFixed(2));

// Row 2
const val1_2 = ref(3);
const val2_2 = ref(0);
const val3_2 = computed(() => (val1_2.value * val2_2.value).toFixed(2));

// Row 3
const val1_3 = ref(4);
const val2_3 = ref(0);
const val3_3 = computed(() => (val1_3.value * val2_3.value).toFixed(2));

// Total Calculation
const total = computed(() => {
  return (
    parseFloat(val3_1.value) +
    parseFloat(val3_2.value) +
    parseFloat(val3_3.value)
  ).toFixed(2); // Ensures 2 decimal places
});
</script>

<style scoped>
input {
  margin: 5px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>

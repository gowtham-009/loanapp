<template>
	<div>
	  <input v-model="formattedDate" type="date">
	</div>
  </template>
  
  <script setup>
  import { ref, watch, onMounted } from 'vue';
  
  const currentdate = new Date();
  const formattedDate = ref(`${currentdate.getFullYear()}-${String(currentdate.getMonth() + 1).padStart(2, '0')}-${String(currentdate.getDate()).padStart(2, '0')}`);
  
  const fun = () => {
	console.log(formattedDate.value);
  };
  
  // Run the function initially when the component is mounted
  onMounted(() => {
	fun();
  });
  
  // Watch for changes in the date input and run the function (runs once per change)
  watch(formattedDate, () => {
	fun();
  });
  </script>
  
  <style lang="scss" scoped>
  </style>
  
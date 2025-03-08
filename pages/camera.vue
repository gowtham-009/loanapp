<script setup>
const count = useState('counter', () => 0)  // Create a global state

const increment = () => {
  count.value++  // Modify the state
  console.log(count.value)
}

</script>

<template>
  <div>
    <h1>Counter: {{ count }}</h1>
    <button @click="increment">Increase</button>
  </div>
</template>

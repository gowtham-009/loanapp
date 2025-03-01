<template>
	<div>
		<form>
			<input type="number" v-model="givenamount" placeholder="Enter Amount">
			<input type="text" v-model="intrestamount" readonly>
		</form>
	</div>
</template>

<script setup>
import { ref, watch } from "vue";

const givenamount = ref(0);
const intrestamount = ref(6666);

// Watch for changes in givenamount and update intrestamount
watch(givenamount, (newVal) => {
    intrestamount.value = newVal * 1.1; // Example: 10% interest
});
</script>

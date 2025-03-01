<template>
    <div>
        <form >
            <input type="text" v-model="loannumber">
            <input type="text" v-model="loanvalue">
            <input type="text"v-model="loantype">
            <input type="text" v-model="loandate">
            <input type="date" v-model="returndate">
            <v-slider v-model="discountRate" thumb-label="always" :min="0" :max="100" :step="1"
            @change="handleDiscountRateInput" />
            <input type="text"  v-model="givenamount" >
            <input type="text" v-model="intrest">
            <input type="text" v-model="offbook">

            <button>{{ totalamount }}</button>
        </form>
    </div>
</template>

<script setup>
import { ref} from 'vue'
const loannumber=ref('LN001')
const loanvalue=ref('20000')
const loantype=ref('Gold')
const loandate=ref('28-12-2024')
const returndate=ref('01-03-2025')
const discountRate=ref(16)
const intrest=ref(0)
const offbook=ref(0)
const totalamount=ref(0)

</script>

<style>

</style>
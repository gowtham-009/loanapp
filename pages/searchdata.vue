<template>
  <div v-if="isAuthenticated">
    <div v-if="errorpopup" style="position: absolute; width: 100%; z-index: 10;">
      <v-alert type="error">
        {{ errormessage }}
      </v-alert>
    </div>
    <div class="w-100 bg-indigo d-flex justify-center" :style="{ height: box1Height + 'px' }"
      style="border: 1px solid red;">
      <logo />
    </div>

    <div class="w-100" :style="{ height: box2Height + 'px' }" style="border: 2px solid yellow;">
      <v-tabs v-model="tab" align-tabs="center">
        <v-tab value="mobile">Mobile Number</v-tab>
        <v-tab value="name">Name</v-tab>
        <v-tab value="proof">Proof ID</v-tab>
      </v-tabs>

      <v-tabs-window v-model="tab">
        <v-tabs-window-item value="mobile">
          <v-form @submit.prevent="filterdata('mobile')" class="pa-1">
            <v-text-field class="mt-1" v-model="phoneNumber" label="Mobile Number" variant="solo-filled"
              @input="validateInput" :rules="[validateLength]" maxlength="10"></v-text-field>
            <v-btn class="bg-deep-purple-accent-4 mt-1" size="x-large" type="submit" block>Search</v-btn>
          </v-form>
        </v-tabs-window-item>

        <v-tabs-window-item value="name">
          <v-form @submit.prevent="filterdata('nameplace')" class="pa-1">
            <v-text-field class="mt-3" v-model="name" variant="solo-filled" type="text" hide-details maxlength="25"
              @keypress="allowOnlyAlphabets($event)" @input="name = name.toUpperCase()"
              label="Name (type min 25 characters)">
            </v-text-field>

            <v-text-field class="mt-2" v-model="place" variant="solo-filled" type="text" hide-details maxlength="25"
              @keypress="allowOnlyAlphabets($event)" @input="place = place.toUpperCase()"
              label="Place (type min 25 characters)">
            </v-text-field>
            <v-btn class="mt-2 bg-deep-purple-accent-4" size="x-large" type="submit" block>Search</v-btn>
          </v-form>
        </v-tabs-window-item>

        <v-tabs-window-item value="proof">
          <v-form @submit.prevent="filterdata('proof')">
            <CheckboxButton v-for="option in options" :key="option.value" :name="'proof-type'" :label="option.label"
              :value="option.value" v-model="selectedOption" class="pa-2" />

            <v-text-field class="mt-2" variant="solo-filled" v-model="proofid" :counter="16" :rules="proofRules"
              label="Proof ID / Number" placeholder="Enter Unique ID Number" hide-details
              @input="proofid = proofid.toUpperCase().replace(/[^A-Z0-9]/g, '').slice(0, 16)"></v-text-field>

            <v-btn class="mt-3 bg-deep-purple-accent-4" size="x-large" type="submit" block>Search</v-btn>
          </v-form>
        </v-tabs-window-item>
      </v-tabs-window>

      <div class="d-flex justify-center" v-if="loading">
      <v-progress-circular color="purple" indeterminate></v-progress-circular>
  </div>

  <div class="w-100 d-flex flex-column ga-2 cont" v-if="datashow">
    <p class="text-center">{{ nodata }}</p>
    <div v-for="globaldata in datauser" :key="globaldata.ID">
      <v-card class="bg-grey-lighten-5 pa-2" variant="tonal">
        <div class="w-100 d-flex justify-space-between pa-2">
          <p class="text-indigo">ID: {{ globaldata.ID }}</p>
          <p class="text-indigo">APP ID: {{ globaldata.App_id }}</p>
        </div>
        <div class="w-100 pa-2">
          <p class="text-black">Name: {{ globaldata.Name }}</p>
          <p class="text-black">Mobile Number: {{ globaldata.MobileNum }}</p>
          <p class="text-black">Address: {{ globaldata.Address1 }}</p>
          <p class="text-black">Proof Type: {{ globaldata.ProofType }}</p>
          <p class="text-black">Proof Details: {{ globaldata.ProofDetails }}</p>
          <p class="text-black">Place: {{ globaldata.Place }}</p>
        </div>
        <div class="w-100 d-flex ga-1">
          <div class="w-100"><v-btn class="bg-yellow" @click="editdata(globaldata.Loan_appid)" block>Edit</v-btn></div>
          <div class="w-100"><v-btn class="bg-indigo" @click="selectloandata(globaldata.App_id)" block>Select</v-btn></div>
        </div>
      </v-card>
    </div>
  </div>
    </div>

  </div>
</template>

<script setup>
import logo from '~/components/logo.vue'
import { ref, watch, computed, onBeforeMount } from 'vue';
import CheckboxButton from '~/components/CheckboxButton.vue';
import { useRouter } from 'vue-router';
import CryptoJS from 'crypto-js';



const isAuthenticated = ref(false)
const errorpopup = ref(false)
const errormessage = ref(null)

const loading=ref(false)
const datashow=ref(true)
const deviceHeight = ref(0)
const box1Height = ref(0)
const box2Height = ref(0)
const tab = ref(null)

const phoneNumber = ref('')

const place = ref('')
const name = ref('')

const proofid=ref('')

    const options = ref([
      { label: 'AADHAR ID', value: 'AADHAR ID' },
      { label: 'VOTER ID', value: 'VOTER ID' },
      { label: 'DRIVING LICENCE', value: 'DRIVING LICENCE' },
      { label: 'RATION CARD', value: 'RATION CARD' },
      { label: 'BANK PASSBOOK', value: 'BANK PASSBOOK' },
      { label: 'PASSPORT', value: 'PASSPORT' },
      { label: 'OTHER', value: 'OTHER' },
      { label: 'DIGI LOCKER', value: 'DIGI LOCKER' },
    ]);

  const selectedOption = ref('');

onBeforeMount(() => {
  const token = localStorage.getItem('token')

  if (!token) {

    router.replace('/login')
  } else {

    isAuthenticated.value = true
  }
})


const updateSizes = () => {
  deviceHeight.value = window.innerHeight
  box1Height.value = deviceHeight.value * 0.10
 box2Height.value = deviceHeight.value *0.90
}


onMounted(() => {
  updateSizes()
  window.addEventListener('resize', updateSizes) // Update sizes on window resize
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', updateSizes) // Clean up event listener
})

const validateInput = () => {
  phoneNumber.value = phoneNumber.value.replace(/\D/g, "").slice(0, 10);
};

const validateLength = () => {
  return phoneNumber.value.length === 10;
};

const allowOnlyAlphabets = (event) => {
  const char = String.fromCharCode(event.keyCode || event.which);
  const pattern = /^[a-zA-Z]+$/;

  if (!pattern.test(char)) {
    event.preventDefault();
  }
};

const datauser=ref([])
const userdata=async()=>{
      loading.value=true
      datashow.value=false
      const apiurl='https://vaanam.w3webtechnologies.co.in/loandb/getlastdata_loan.php'
      
      
      try {
        const response=await fetch(apiurl,{
          method:"GET",
          
        })
        if(!response.ok){
          throw new Error('Failed your response try again!');
          }
          else{
            const data=await response.json()   
            datauser.value=data  
          }
        } catch (error) {
          errorpopup.value=true
    errormessage.value = error.message
        }
        finally{
          loading.value=false
          datashow.value=true
        }

  }

  userdata()

  const filterdata=async(filtervalue)=>{
    loading.value=true
    datashow.value=false
    const apiurl='https://vaanam.w3webtechnologies.co.in/loandb/loan_search_data.php'
    const formdata=new FormData()
    if(filtervalue=='mobile'){
      formdata.append('mobilenumber', phoneNumber.value)
      formdata.append('place', '')
    }
    else if(filtervalue=='nameplace'){
      formdata.append('name', name.value)
      formdata.append('place', place.value)
    }
    else if(filtervalue=='proof'){
      formdata.append('proofid', proofid.value)
      formdata.append('prooftype', selectedOption.value)
    }
    try {
      const response=await fetch(apiurl,{
          method:"POST",
          body:formdata
        })

        if(!response.ok){
          throw new Error('Failed your response try again!');
          }
          else{
            const data=await response.json()
            datauser.value=data
           
          }

    } catch (error) {
      errorpopup.value=true
      errormessage.value = error.message
    }
    finally{
      loading.value=false
    datashow.value=true
    }

  }
  const router=useRouter()
  const secretKey = "appidsecreatekey001";
  const editdata=(loanid)=>{
    const encryptedValue = CryptoJS.AES.encrypt(loanid, secretKey).toString();
    router.push({ path: '/new_loan', query: { data: encodeURIComponent(encryptedValue),  enable: 'true' } });
  }

</script>

<style>
.cont {
  flex-grow: 1;
  overflow-y: auto;
  min-height: 300px; /* Set a reasonable minimum height */
  max-height: calc(100vh - 150px); /* Adjust based on header/footer heights */
}
</style>
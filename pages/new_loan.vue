<template>
  <div v-if="isAuthenticated">
    <div class="d-flex justify-center h-screen d-flex justify-center align-center" v-if="loading">
      <v-progress-circular color="purple" indeterminate></v-progress-circular>
    </div>
    <div v-if="errorpopup" style="position: absolute; width: 100%; z-index: 10;">
      <v-alert type="error" @click="closepopup()">
        {{ errormessage }}
      </v-alert>

    </div>
    <div v-if="successpopup" style="position: absolute; width: 100%; z-index: 10;">
      <v-alert type="success">
        {{ successmesage }}
      </v-alert>
    </div>
    <div v-if="loanform">
      <div class="bg-blue d-flex justify-center align-center" :style="{ height: box1Height + 'px' }">

      </div>

      <div class="d-flex justify-center align-center " :style="{ height: box2Height + 'px' }">
        <v-btn class="bg-yellow" block @click="handleButtonClick">{{ buttonLabel }}</v-btn>
      </div>

      <v-form @submit.prevent :disabled="formDisabled" :class="{ 'disabled-box': formDisabled }"
        :style="{ height: form + 'px', backgroundColor: formColor }" class="w-100">
        <div class="d-flex flex-column justify-space-between" :style="{ height: box3Height + 'px' }">
          <div>
            <div class="w-100 d-flex">
              <div class="w-100 pa-1">
                <v-text-field variant="solo" v-model="sfa" label="SFA" type="number" hide-details readonly />
              </div>
              <div class="w-100 pa-1">
                <p class="text-blue text-center text-h6 mt-2">New Loan</p>
              </div>
              <div class="w-100 pa-1">
                <v-text-field variant="solo" v-model="currentdate"  type="date" hide-details readonly />
                
              </div>
            </div>

            <div class="options" style="display: flex; justify-content: center">
              <CheckboxButton v-for="option in options" :key="option.value" :name="'check-type6'" :label="option.label"
                :value="option.value" v-model="selectedOption" class="pa-2" @click="updateFormColor" />
            </div>

            <div class="w-100 d-flex ga-2">
              <v-text-field variant="solo" v-model="loanvalue" label="Loan Value:" placeholder="0.00" type="text"
                class="w-100" @input="formatLoanValue" />

              <v-text-field variant="solo" v-model="handover" label="Handover Value:" placeholder="0.00" type="text"
                class="w-100" @input="formathandover" />

            </div>

            <div class="w-100">
              <v-text-field variant="solo" v-model="weight" label="weight Value:" placeholder="0.00" type="text"
                class="w-100" @input="formatweight" />

              <v-text-field variant="solo" v-model="powndetails" label="Pown Details" placeholder="Powndetails"
                type="text" class="w-100" />
            </div>

            <div class="w-100 pa-1 d-flex justify-center align-center flex-column" v-if="cmabox">
              <div class="w-100 d-flex justify-start">
                <p class="text-left">Capture Proof</p>
              </div>
              <!-- Open Camera Button -->
              <div v-if="cameraoperation" :style="{ height: cmaheight + 'px' }"
                class="w-100 rounded bg-green-darken-4 d-flex justify-center align-center">
                <p class="text-center" @click="openCamera">
                  <i class="mdi mdi-camera-flip-outline"></i> Open Camera/Upload Files
                </p>
              </div>
              <!-- Camera Container -->
              <div v-if="cameracontainer" class="w-100 rounded d-flex justify-center align-center flex-column">
                <video ref="videoElement" :style="{ height: cmaheight + 'px' }" autoplay class="w-100"
                  style="object-fit: cover; border-radius: 5px;"></video>
                <div class="w-100 d-flex ga-2 justify-center">
                  <v-btn prepend-icon="mdi mdi-camera-flip-outline" @click="flipCamera" class="mt-2 bg-green-darken-4"
                    text="Flip" variant="flat"></v-btn>
                  <v-btn prepend-icon="mdi mdi-camera-enhance" @click="captureImage" class="mt-2 bg-gray-darken-4"
                    text="Capture" variant="flat"></v-btn>
                </div>
              </div>
              <!-- Display Captured Image -->
              <div v-if="capturedImage" class="w-100">
                <img :src="capturedImage" alt="Captured Photo" :style="{ height: cmaheight + 'px' }"
                  class="w-100 rounded shadow-lg" />
                <div class="w-100 d-flex ga-2 justify-center">
                  <v-btn prepend-icon="mdi mdi-camera-flip-outline" @click="dialog = true"
                    class="mt-2 bg-indigo-darken-4" text="Expand" variant="flat"></v-btn>
                  <v-btn prepend-icon="mdi mdi-camera-enhance" @click="recapturephoto" class="mt-2 bg-gray-darken-4"
                    text="Re-Capture" variant="flat"></v-btn>
                </div>
              </div>
              <canvas ref="canvasElement" style="display: none;"></canvas>
            </div>

            <div class="w-100 pa-2" v-if="showphoto">
              <img :src="capturedImage" :style="{ height: cmaheight + 'px' }" alt="Captured Photo"
                class="w-100 rounded shadow-lg" />
              <v-btn block @click="retake()" class="bg-red text-white">Re Take</v-btn>
            </div>
          </div>
          <!-- <span>vnaeorn{{ appid }}</span> -->
          <div class="d-flex ga-2">
            <div class="w-100 d-flex justify-center align-end">
              <v-btn @click="reset()" class="bg-yellow text-black" block>Reset</v-btn>
            </div>

            <div class="w-100 d-flex justify-center align-end" v-if="updateb">
              <v-btn block type="submit" @click="handleEvent('update')" class="bg-blue text-white">
                Update
              </v-btn>

            </div>
            <div class="w-100 d-flex justify-center align-end" v-if="insertb">

              <v-btn block type="submit" @click="handleEvent('insert')" class="bg-green text-white">
                Next
              </v-btn>
            </div>
          </div>

        </div>
      </v-form>

      <v-dialog v-model="dialog" width="auto">
        <v-card max-width="400">
          <img :src="capturedImage" alt="Captured Photo" class="w-100 rounded shadow-lg" />
        </v-card>
      </v-dialog>
    </div>
  </div>

</template>

<script setup>
import { ref, watch, computed, onBeforeMount } from 'vue';
import CheckboxButton from '~/components/CheckboxButton.vue'; // Adjust the path as necessary
import { useRouter, useRoute } from 'vue-router'
import CryptoJS from 'crypto-js';

const isAuthenticated = ref(false)
const deviceHeight = ref(0)
const box1Height = ref(0)
const box2Height = ref(0)
const box3Height = ref(0)
const cmaheight = ref(0)
const cmabox = ref(true)
const showphoto = ref(false)
const loanform = ref(true)
const loading = ref(false)

const successpopup=ref(false)
const successmesage=ref('')

const insertb = ref(true)
const updateb = ref(false)

const cameraoperation = ref(true);
const cameracontainer = ref(false);
const videoElement = ref(null);
const canvasElement = ref(null);
const capturedImage = ref(null);
const dialog = ref(false)


const formDisabled = ref(true);
const buttonLabel = ref("Search User");
const router = useRouter();
const route = useRoute();

const getCurrentDate = () => {
  const date = new Date();
  const year = date.getFullYear();
  const month = String(date.getMonth() + 1).padStart(2, '0');
  const day = String(date.getDate()).padStart(2, '0');
  return `${year}-${month}-${day}`;
};

const errorpopup = ref(false)
const errormessage = ref(null)

const sfa = ref(1000)
const currentdate = ref(getCurrentDate());
const selectedOption = ref('');
const loanvalue = ref('')
const handover = ref('')
const weight = ref('')
const powndetails = ref('')

const options = ref([
  { label: 'Gold', value: 'Gold' },
  { label: 'Silver', value: 'Silver' },
  { label: 'Brass', value: 'Brass' },
]);

const formColor = computed(() => {
  switch (selectedOption.value) {
    case 'Gold':
      return '#fbfaea';
    case 'Silver':
      return '#f2f2f2';
    case 'Brass':
      return '#e8e29a';
    default:
      return 'white';
  }
});

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
  box1Height.value = deviceHeight.value * 0.05 // 8% height
  box2Height.value = deviceHeight.value * 0.05 // 92% height
  box3Height.value = deviceHeight.value * 0.90 // 92% height
  cmaheight.value = box3Height.value * 0.3 // 92% height


}

onMounted(() => {
  updateSizes()
  window.addEventListener('resize', updateSizes) // Update sizes on window resize
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', updateSizes) // Clean up event listener
})



const facingMode = ref("user"); // "user" for front camera, "environment" for back camera
let stream = null;

const openCamera = async () => {
  cameraoperation.value = false;
  cameracontainer.value = true;
  capturedImage.value = null;
  await startCamera();
};

const startCamera = async () => {
  try {
    if (stream) {
      stream.getTracks().forEach(track => track.stop());
    }
    stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: facingMode.value } });
    if (videoElement.value) {
      videoElement.value.srcObject = stream;
    }
  } catch (error) {
    console.error("Error accessing camera:", error);
  }
};

const flipCamera = async () => {
  facingMode.value = facingMode.value === "user" ? "environment" : "user";
  await startCamera();
};

const captureImage = () => {
  if (!videoElement.value || !canvasElement.value) return;
  const canvas = canvasElement.value;
  const video = videoElement.value;
  canvas.width = video.videoWidth;
  canvas.height = video.videoHeight;
  const ctx = canvas.getContext('2d');
  ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
  capturedImage.value = canvas.toDataURL('image/png');
  cameracontainer.value = false;
};

const recapturephoto = async () => {
  capturedImage.value = null;
  cameraoperation.value = false;
  cameracontainer.value = true;
  await startCamera();
};

onBeforeMount(() => {
  const token = localStorage.getItem('token');

  if (!token) {
    router.replace('/login');
  } else {
    isAuthenticated.value = true;
  }
});

onBeforeUnmount(() => {
  if (stream) {
    stream.getTracks().forEach(track => track.stop());
  }
});

const handleButtonClick = () => {
  if (formDisabled.value) {
    // Enable the form
    formDisabled.value = false;
    buttonLabel.value = "Clear"; // Change the button label to Clear
    buttonLabel.value = "Search User"; // Change the button label back
    router.push({ name: 'search2' });
  } else {
    // Clear the form inputs (resetting to empty)
    document.querySelectorAll('input[type="text"]').forEach(input => input.value = '');
    // Optionally, disable the form again
    formDisabled.value = true;
    buttonLabel.value = "Search User";

  }
};



const formatLoanValue = () => {
  let value = loanvalue.value.replace(/[^0-9.]/g, ''); // Remove non-numeric characters except '.'
  let parts = value.split('.');

  if (parts.length > 2) {
    value = parts[0] + '.' + parts.slice(1).join('');
  }
  if (parts[1] && parts[1].length > 2) {
    value = parts[0] + '.' + parts[1].slice(0, 2);
  }

  loanvalue.value = value;
};

const formathandover = () => {
  let value = handover.value.replace(/[^0-9.]/g, ''); // Remove non-numeric characters except '.'
  let parts = value.split('.');

  if (parts.length > 2) {
    value = parts[0] + '.' + parts.slice(1).join('');
  }
  if (parts[1] && parts[1].length > 2) {
    value = parts[0] + '.' + parts[1].slice(0, 2);
  }

  handover.value = value;
};


const formatweight = () => {
  let value = weight.value.replace(/[^0-9.]/g, ''); // Remove non-numeric characters except '.'
  let parts = value.split('.');

  if (parts.length > 2) {
    value = parts[0] + '.' + parts.slice(1).join('');
  }
  if (parts[1] && parts[1].length > 2) {
    value = parts[0] + '.' + parts[1].slice(0, 2);
  }

  weight.value = value;
};


const getloannumber = async () => {

  const apiurl_loan = 'https://vaanam.w3webtechnologies.co.in/loandb/getloan_number.php'



  try {
    const response = await fetch(apiurl_loan, {
      method: "GET",

    })
    if (!response.ok) {
      throw new Error('Failed your response try again!');
    }
    else {

      const data = await response.json()
      const loan_no = Number(data[0].Loan_Number) + 1


      sfa.value = loan_no


    }
  } catch (error) {
    console.log(error.message)
  }


}

const secretKey = "appidsecreatekey001";
const decryptedValue = ref('');


onMounted(() => {
  if (route.query.enable === 'true') {

    formDisabled.value = false;
    buttonLabel.value = "Clear";

    showphoto.value = false

  }

  const encryptedData = route.query.data;
  if (encryptedData) {
    const bytes = CryptoJS.AES.decrypt(decodeURIComponent(encryptedData), secretKey);
    decryptedValue.value = bytes.toString(CryptoJS.enc.Utf8);

    const Loan_appid = decryptedValue.value
    if (Loan_appid.includes('L')) {
      updateb.value = true
      insertb.value = false
      getloandata(decryptedValue.value)
    }


  }
});

const handleEvent = (sendmode) => {
  if (sendmode == 'insert') {


    if (!currentdate) {
      errorpopup.value = true
      errormessage.value = 'Date is Required'
    }
    else if (!selectedOption.value) {
      errorpopup.value = true
      errormessage.value = 'Select Loan Type'
    }
    else if (!loanvalue.value) {

      errorpopup.value = true
      errormessage.value = 'Loan value is Required'
    }
    else if (!handover.value) {
      errorpopup.value = true
      errormessage.value = 'Handover value is Required'
    }
    else if (!weight.value) {
      errorpopup.value = true
      errormessage.value = 'Weight value is Required'
    }
    else if (!powndetails.value) {
      errorpopup.value = true
      errormessage.value = 'Pown details is Required'
    }
    else if (!capturedImage.value) {
      errorpopup.value = true
      errormessage.value = 'Photo is Required'
    }

    else {
      errorpopup.value = false
      errormessage.value = ''

      postdata_loan()
    }
  }

  if (sendmode == 'update') {


    if (!currentdate) {
      errorpopup.value = true
      errormessage.value = 'Date is Required'
    }
    else if (!selectedOption.value) {
      errorpopup.value = true
      errormessage.value = 'Select Loan Type'
    }
    else if (!loanvalue.value) {

      errorpopup.value = true
      errormessage.value = 'Loan value is Required'
    }
    else if (!handover.value) {
      errorpopup.value = true
      errormessage.value = 'Handover value is Required'
    }
    else if (!weight.value) {
      errorpopup.value = true
      errormessage.value = 'Weight value is Required'
    }
    else if (!powndetails.value) {
      errorpopup.value = true
      errormessage.value = 'Pown details is Required'
    }
    else if (!capturedImage.value) {
      errorpopup.value = true
      errormessage.value = 'Photo is Required'
    }

    else {
      errorpopup.value = false
      errormessage.value = ''

      update_data()
    }
  }

}






if (isAuthenticated.value == false) {

  getloannumber()
}

const getFormattedDateTime = () => {
  const date = new Date();
  const formattedDate = date.toISOString().split('T')[0]; // dd-mm-yyyy
  const formattedTime = date.toLocaleTimeString('en-GB', { hour12: false }); // hh:mm:ss
  return `${formattedDate} ${formattedTime}`;
};

const currentDateTime = ref(getFormattedDateTime());


const postdata_loan = async () => {

  loading.value = true
  loanform.value = false
  errormessage.value = null
  const apiurl_loan = 'https://vaanam.w3webtechnologies.co.in/loandb/loan_update.php'
  const formdata_loan = new FormData()
  formdata_loan.append('appid', decryptedValue.value)
  formdata_loan.append('datetime', currentDateTime.value);
  formdata_loan.append('loannumber', sfa.value)
  formdata_loan.append('loandate', currentdate.value)
  formdata_loan.append('loantype', selectedOption.value)
  formdata_loan.append('loanvalue', loanvalue.value)
  formdata_loan.append('loanhandover', handover.value)
  formdata_loan.append('weight', weight.value)
  formdata_loan.append('pawndetails', powndetails.value)
  formdata_loan.append('nporul', capturedImage.value)


  try {
    const response_loan = await fetch(apiurl_loan, {
      method: "POST",
      body: formdata_loan
    })
    if (!response_loan.ok) {
      throw new Error('Failed your response try again!');
    }
    else {

      const data_loan = await response_loan.json()

    }
  } catch (error) {
    errorpopup.value = false
    errormessage.value = error.message
  }
  finally {
    loading.value = false
    loanform.value = true
     successpopup.value=true
    successmesage.value='Successfully Inserted'

    setInterval(() => {
      successpopup.value=false
    }, 1000);

  }
}
const reset = () => {
  loanvalue.value = ''
  handover.value = ''
  weight.value = ''
  powndetails.value = ''
  capturedImage.value = ''
  formDisabled.value = true;
  buttonLabel.value = "Search user";
  cameracontainer.value = true
  capturedImage.value = false

}

const getloandata = async (loanval) => {


  if (loanval) {
    cmabox.value = false
    cameracontainer.value = false
    showphoto.value = true
  }

  errormessage.value = null
  const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/loan_getdata.php';
  const formdata_loan = new FormData();
  formdata_loan.append('loanid', decryptedValue.value);

  try {
    const response = await fetch(apiurl, {
      method: "POST",
      body: formdata_loan,
    });

    if (!response.ok) {
      throw new Error('Failed your response try again!');
    } else {
      const data_loan = await response.json();
      sfa.value = data_loan[0].Loan_Number;
      currentdate.value = data_loan[0].Loan_Date;
      selectedOption.value = data_loan[0].Loan_Type;
      loanvalue.value = data_loan[0].Loan_Value;
      handover.value = data_loan[0].Loan_Handover;
      weight.value = data_loan[0].Weight;
      powndetails.value = data_loan[0].Pawn_Details;
      capturedImage.value = data_loan[0].nPorul

    }
  } catch (error) {
    errorpopup.value = false
    errormessage.value = error.message
  } finally {

  }

}



const retake = () => {
  capturedImage.value = ''
  showphoto.value = false
  cmabox.value = true
}


const update_data = async () => {
  loading.value = true;
  loanform.value = false

  const formdata = new FormData();
  const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/loan_edit_data.php';
  formdata.append('datetime', currentDateTime.value);
  formdata.append('loanid', decryptedValue.value);
  formdata.append('loannumber', sfa.value);
  formdata.append('loandate', currentdate.value);
  formdata.append('loantype', selectedOption.value);
  formdata.append('loanvalue', loanvalue.value);
  formdata.append('loanhandover', handover.value);
  formdata.append('weight', weight.value);
  formdata.append('pawndetails', powndetails.value);
  formdata.append('nporul', capturedImage.value);



  try {
    const response = await fetch(apiurl, {
      method: "POST",
      body: formdata
    });

    if (!response.ok) {
      throw new Error('Failed to update data, please try again.');
    } else {
      const data = await response.json();
      // Handle the response data as needed
    }
  } catch (error) {
    errorpopup.value = false
    errormessage.value = error.message
  } finally {
    loading.value = false;
    loanform.value = true
     successpopup.value=true
    successmesage.value='Successfully Updated'

    setTimeout(() => {
      successpopup.value=false
    }, 1000);
  }
}

const closepopup=()=>{
  closepopup.value=false
}
</script>

<style>
.disabled-box {
  pointer-events: none;
  opacity: 0.5;
}
</style>
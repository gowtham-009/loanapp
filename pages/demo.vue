<template>
  <div v-if="isAuthenticated">
    <div class="w-100" v-if="errorpopup">
      <v-alert type="error">{{ errormessage }}
      </v-alert>
    </div>


    <div class="d-flex bg-blue justify-center align-center" :style="{ height: box1Height + 'px' }">

    </div>

    <v-form>
      <div class="d-flex justify-space-between flex-column pa-1" :style="{ height: box2Height + 'px' }">
        <div>
          <p class="text-indigo text-center "><b>Enter Closing Details {{ userappid }}</b></p>

          <div class="w-100 d-flex ga-2 mt-1">
            <v-text-field v-model="loannumber" label="Loan Number" variant="solo-filled" readonly></v-text-field>
            <v-text-field v-model="loantype" label="Loan Type" variant="solo-filled" readonly></v-text-field>
          </div>

          <div class="w-100 d-flex ga-2 mt-1">
            <v-text-field v-model="loandate" label="Loan Date" variant="solo-filled" readonly></v-text-field>
            <v-text-field v-model="loanvalue" label="Loan Value" variant="solo-filled" readonly></v-text-field>
          </div>

          <div class="w-100 mt-1">
            <span>Return Date</span>
            <input v-model="currentdate" type="date" readonly
              class="w-100 p-2 px-2 rounded-lg shadow border  bg-grey-lighten-4 text-gray-700"
              style="outline: none; height: 50px;">
          </div>

          <div class="w-100 mt-2">
            <span>Interest %</span>
            <v-slider v-model="discountRate" thumb-label="always" :min="0" :max="100" :step="1" />
          </div>

          <div class="w-100 d-flex ga-2">
            <v-text-field label="Given Amount" type="number" v-model="givenamount" variant="solo-filled"></v-text-field>
          </div>

          <v-row no-gutters>
            <v-col class="pa-1" cols="6">
              <p>Interest Amount</p>
              <p>{{ interestamount }}</p>
            </v-col>
            <v-col class="pa-1" cols="6">
              <v-text-field variant="solo-filled" type="number" v-model="in_amount" readonly></v-text-field>
            </v-col>
          </v-row>

          <v-row no-gutters>
            <v-col class="pa-1" cols="6">
              <p>OffBook</p>
              <p>{{ offbook }}</p>
            </v-col>
            <v-col class="pa-1" cols="6">
              <v-text-field variant="solo-filled" type="number" v-model="offbook" readonly></v-text-field>
            </v-col>
          </v-row>
        </div>

        <div class="w-100 pa-1  d-flex ga-1">
          <div class="w-100"><v-btn type="button" @click="sheet = !sheet" class="btn bg-blue" block>{{ total }}</v-btn>
          </div>
          <div class="w-100"><v-btn @click="dialog = true" class="btn bg-green" block>Submit & Close</v-btn></div>
        </div>
      </div>
    </v-form>


    <v-bottom-sheet v-model="sheet">
      <v-card height="500">
        <v-tabs v-model="tab" align-tabs="center">
        <v-tab value="one">Saved</v-tab>
        <v-tab value="two"> Months</v-tab>
        <v-tab value="three">Interest</v-tab>
        <v-tab value="four">Collect</v-tab>
      </v-tabs>

      <v-tabs-window v-model="tab">
        <v-tabs-window-item value="one" class="pa-2">
          <v-carousel>
                    <v-carousel-item>
                      <div class="w-100 d-flex justify-space-between">
                        <span class="text-gray">Loan Details:</span>
                        <span class="text-gray">{{ loandetails }}=> {{ weight }}g</span>
                      </div>
                      <div class="w-100 d-flex justify-end">
                        <span class="text-gray">{{ loannum }}</span>
                      </div>
                      <div class="w-100 mt-2  d-flex justify-space-between">
                        <span class="text-gray">Name:</span>
                        <span class="text-gray">{{ name }}</span>
                      </div>
                      <div class="w-100  d-flex justify-space-between">
                        <span class="text-gray">Mobile Number:</span>
                        <span class="text-gray">{{ mobileno }}</span>
                      </div>
                      <div class="w-100 mt-2 d-flex justify-space-between">
                        <span class="text-gray">ID Details:</span>
                        <span class="text-gray">{{ iddetails }}</span>
                      </div>
                      <div class="w-100 d-flex justify-space-between">
                        <span class="text-gray">ID Number:</span>
                        <span class="text-gray">{{ idnumber }}</span>
                      </div>

                      <div class="w-100 d-flex justify-start mt-2">
                        <p>Address:</p>
                      </div>
                      <div class="w-100 d-flex justify-start ">
                        <p>{{address}}</p>
                      </div>
                    </v-carousel-item>

                    <v-carousel-item>
                      <span>Nproof</span>
                      <div class="w-100 p">
                        <img :src=nproof alt="">
                      </div>

                    </v-carousel-item>

                    <v-carousel-item>
                      <span>Nperson</span>
                      <div class="w-100 p">
                        <img :src=nperson alt="">
                      </div>
                    </v-carousel-item>

                    <v-carousel-item>
                      <span>Nporul</span>
                      <div class="w-100 p">
                        <img :src=nproul alt="">
                      </div>
                    </v-carousel-item>
                  </v-carousel>


        </v-tabs-window-item>

        <v-tabs-window-item value="two">
          Two
        </v-tabs-window-item>

        <v-tabs-window-item value="three">
          Three
        </v-tabs-window-item>
        <v-tabs-window-item value="four" class="pa-2">
       </v-tabs-window-item>
      </v-tabs-window>
      </v-card>
    </v-bottom-sheet>
  </div>
</template>
<script setup>
import { ref, onMounted, watch, onBeforeMount } from 'vue';
import CryptoJS from 'crypto-js';
import { useRoute, useRouter } from 'vue-router';

const router = useRouter();
const route = useRoute();

const isAuthenticated = ref(false);
const errorpopup = ref(false);
const errormessage = ref(null);

const sheet = ref(false)
const tab=ref(null)

const deviceHeight = ref(0);
const box1Height = ref(0);
const box2Height = ref(0);

const userappid = ref('');
const currentdate = ref(new Date().toISOString().split('T')[0]);
const loannumber = ref('');
const loantype = ref('');
const loandate = ref('');
const loanvalue = ref('');
const discountRate = ref('');
const givenamount = ref(0);
const interestamount = ref(0);
const offbook = ref(0);
const in_amount = ref(0)

const total = ref('')
const updateSizes = () => {
  deviceHeight.value = window.innerHeight;
  box1Height.value = deviceHeight.value * 0.05;
  box2Height.value = deviceHeight.value * 0.95;
};


onMounted(() => {
  updateSizes();
  window.addEventListener('resize', updateSizes);
});

onBeforeUnmount(() => {
  window.removeEventListener('resize', updateSizes);
});

onBeforeMount(() => {
  const token = localStorage.getItem('token');

  if (!token) {
    router.replace('/login');
  } else {
    isAuthenticated.value = true;
  }
});

const secretKey = "appidsecreatekey001";
const decryptedValue = ref('');

onMounted(() => {
  const encryptedData = route.query.data;
  if (encryptedData) {
    const bytes = CryptoJS.AES.decrypt(decodeURIComponent(encryptedData), secretKey);
    decryptedValue.value = bytes.toString(CryptoJS.enc.Utf8);
    getloandetails(decryptedValue.value);
  }
});

const formatDate = (dateString) => {
  const [year, month, day] = dateString.split('-');
  return `${day}-${month}-${year}`;
};
const loandetails=ref('')
const loannum=ref('')
const name=ref('')
const mobileno=ref('')
const iddetails=ref('')
const idnumber=ref('')
const address=ref('')
const nproof=ref('')
const nperson=ref('')
const nporul=ref('')


const getloandetails = async () => {
  const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/get_loanrow.php';
  const formdata = new FormData();
  formdata.append('loannumber', decryptedValue.value);
  try {
    const response = await fetch(apiurl, { method: 'POST', body: formdata });
    if (!response.ok) {
      throw new Error('Failed to fetch response. Try again!');
    } else {
      const data = await response.json();
      userappid.value = data[0].App_id;
      loannumber.value = data[0].Loan_Number;
      loantype.value = data[0].Loan_Type;
      loandate.value = formatDate(data[0].Loan_Date);
      loanvalue.value = data[0].Loan_Value;
      loannum.value=data[0].Loan_appid
      loandetails.value=data[0].Pawn_Details
name.value=data[0].Name
mobileno.value=data[0].MobileNum
iddetails.value=data[0].ProofType
idnumber.value=data[0].ProofDtails
address.value=data[0].Address1
nproof.value=data[0].nProof
nperson.value=data[0].nPerson
nporul.value=data[0].nPorul
      interestval(data);
      calculation(loandate.value);
    }
  } catch (error) {
    errorpopup.value = true;
    errormessage.value = error.message;
  }
};

const interestval = async (loanData) => {
  const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/calculate_condition.php';
  const formdata = new FormData();
  formdata.append('loantype', loanData[0].Loan_Type);
  formdata.append('loanvalue', loanData[0].Loan_Value);

  try {
    const response = await fetch(apiurl, { method: 'POST', body: formdata });
    if (!response.ok) throw new Error('Failed to fetch response. Try again!');
    const data = await response.json();
    discountRate.value = data.DiscountInterest;
  } catch (error) {
    errorpopup.value = true;
    errormessage.value = error.message;
  }
};

// Watch for changes in loanvalue, discountRate, loandate, and currentdate
watch([loanvalue, discountRate, givenamount, loandate, currentdate], () => {
  calculation(loandate.value);
}
);


watch(givenamount, () => {


  if (total.value > givenamount.value) {

    in_amount.value = (givenamount.value - loanvalue.value).toFixed(2)
    offbook.value = 0

  }
  if (total.value == givenamount.value) {
    in_amount.value = interestamount.value
    offbook.value = 0

  }

  if (total.value < givenamount.value) {
    in_amount.value = interestamount.value
    const off = givenamount.value - loanvalue.value
    const offbal = off - in_amount.value

    offbook.value = offbal.toFixed(2)
  }

});

const calculation = (loandate) => {
  const loanamount = parseInt(loanvalue.value);

  const discountrate = discountRate.value;

  const loanDate = formatDater(loandate);
  const currentDate = formatDater(currentdate.value);
  const timeDifference = currentDate.getTime() - loanDate.getTime();
  const dayDifference = Math.ceil(timeDifference / (1000 * 60 * 60 * 24));

  const monthly_intrest = (loanamount * (discountrate / 100) / 12).toFixed(2)

  const no_of_month = Math.round(dayDifference / 30)

  const totalmonths = no_of_month + 0.5
  const totalintrest = parseFloat((totalmonths * monthly_intrest).toFixed(2))

  const finalamount = loanamount + totalintrest

  total.value = finalamount
  interestamount.value = totalintrest




};



const formatDater = (dateString) => {
  if (!dateString) return null;

  let parts;


  if (dateString.includes('-')) {
    parts = dateString.split('-');
    if (parts[0].length === 4) {

      return new Date(parts[0], parts[1] - 1, parts[2]);
    } else {

      return new Date(parts[2], parts[1] - 1, parts[0]);
    }
  }

  return new Date(dateString);
};


//bottom sheet


</script>

<style></style>
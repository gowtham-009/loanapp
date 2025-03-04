<template>
  <div v-if="isAuthenticated">
    <div class="w-100" v-if="errorpopup">
      <v-alert type="error">{{ errormessage }}
      </v-alert>
    </div>
    <div class="w-100" v-if="loading">
      <v-progress-linear indeterminate color="blue"></v-progress-linear>
    </div>
    <div class="w-100" v-if="content">
      
    <div class="d-flex bg-blue justify-center align-center" :style="{ height: box1Height + 'px' }">

</div>

<v-form @submit.prevent="handlEvent">
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
      <div class="w-100"><v-btn type="submit" class="btn bg-green" block>Submit & Close</v-btn></div>
    </div>
  </div>
</v-form>


<v-bottom-sheet v-model="sheet">
  <v-card height="500">
    <v-tabs v-model="tab" align-tabs="center">
    <v-tab value="one">Saved <br>Details</v-tab>
    <v-tab value="two"> Months<br>{{ totmonth }}</v-tab>
    <v-tab value="three">Interest<br>₹{{ interestamount }}</v-tab>
    <v-tab value="four">Collect<br>₹{{ total }}</v-tab>
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
                  <div class="w-100 pa-1">
                    <img :src=nproof alt=""  width="100%" height="100%">
                  </div>

                </v-carousel-item>

                <v-carousel-item>
                  <span>Nperson</span>
                  <div class="w-100 pa-1">
                    <img :src=nperson alt="" width="100%" height="100%">
                  </div>
                </v-carousel-item>

                <v-carousel-item>
                  <span>Nporul</span>
                  <div class="w-100 pa-1">
                    <img :src=nporul alt="" width="100%" height="100%">
                  </div>
                </v-carousel-item>
              </v-carousel>


    </v-tabs-window-item>

    <v-tabs-window-item value="two">
      <v-row no-gutters>
                <v-col class="pa-1" cols="4">
                  <div class="w-100 bg-white rounded-lg" style="height: 80px;">
                    <p class="mt-1 text-h6 text-pink-darken-4">Today Date</p>

                    <p class="text-indigo " style="font-size: 18px;">{{ formatDate(currentdate) }}</p>
                    <span class="pa-1 text-indigo">(-)</span>
                  </div>
                </v-col>
                <v-col class="pa-1" cols="4">
                  <div class="w-100 bg-white rounded-lg " style="height: 80px;">
                    <p class="mt-1 text-h6 text-pink-darken-4">Loan Date</p>
                    <p class="text-indigo " style="font-size: 18px;">{{ loandate }} </p>
                    <span class="pa-1 text-indigo">(=)</span>
                  </div>
                </v-col>
                <v-col class="pa-1" cols="4">
                  <div class="w-100 bg-white rounded-lg " style="height: 80px;">
                    <p class="mt-1 text-h6 text-pink-darken-4">Difference</p>
                    <p class="text-indigo " style="font-size: 18px;">{{ days }} <br><span>Days</span></p>
                  </div>
                </v-col>
              </v-row>
              <v-row no-gutters>
                <v-col class="pa-1" cols="12">
                  <div class="w-100 pa-1 bg-pink-darken-3 rounded-lg ">
                    <p class="text-h6">That is = <span>{{ monthsval }}</span> Months & <span>{{ daysval }}</span>
                      Days</p>
                  </div>
                </v-col>
              </v-row>
              <v-row no-gutters>
                <v-col class="pa-1" cols="4">
                  <div class="w-100 bg-white rounded-lg " style="height: 90px;">
                    <span class="mt-2 text-pink-darken-4" style="font-size: 18px;">Rounded <br> Months</span><br>
                    <div class="w-100 bg-indigo" style="height: 5px;"></div>
                    <span class="text-h6 text-indigo"><b>{{ roundmonths }} (-)</b></span>
                    <div class="w-100 bg-indigo" style="height: 5px;"></div>
                  </div>
                </v-col>
                <v-col class="pa-1" cols="4">
                  <div class="w-100 bg-white rounded-lg" style="height: 90px;">
                    <div class="w-100 bg-white rounded-lg " style="height: 90px;">
                      <span class="mt-2 text-pink-darken-4" style="font-size: 18px;">Advance <br> Months</span><br>
                      <div class="w-100 bg-indigo" style="height: 5px;"></div>
                      <span class="text-h6 text-indigo"><b>{{ advmonth }}.00 (=)</b></span>
                      <div class="w-100 bg-indigo" style="height: 5px;"></div>
                    </div>
                  </div>
                </v-col>
                <v-col class="pa-1" cols="4">
                  <div class="w-100 bg-white rounded-lg" style="height: 90px;">
                    <div class="w-100 bg-white rounded-lg " style="height: 90px;">
                      <span class="mt-2 text-pink-darken-4" style="font-size: 18px;">Total <br> Months</span><br>
                      <div class="w-100 bg-indigo" style="height: 5px;"></div>
                      <span class="text-h6 text-indigo"><b>{{ totmonth }}</b></span>
                      <div class="w-100 bg-indigo" style="height: 5px;"></div>
                    </div>
                  </div>
                </v-col>
              </v-row>
    </v-tabs-window-item>

    <v-tabs-window-item value="three">
      <div class="w-100 pa bg-white rounded-lg" style="border-bottom: 3px solid rebeccapurple;">
                    <p class="text-h6 text-pink-darken-4">Discount Calculation @ <span>{{ discountRate }}.00</span>
                      Paisa</p>
                  </div>
                  <div class="w-100 mt-2 d-flex ga-2">
                    <div class="w-25 pa-1 bg-white rounded-lg">
                      <p class="text-left text-h6 text-pink-darken-4">{{ loanvalue }}</p>

                      <p class="text-left text-h6 text-pink-darken-4">X {{discountRate}}</p>

                      <div class="w-100 bg-pink-darken-4" style="height: 5px;"></div>
                      <p class="text-left text-h6 text-pink-darken-4">{{perval}}</p>
                      <div class="w-100 bg-pink-darken-4" style="height: 5px;"></div>
                    </div>
                    <div class="w-25 pa-1 bg-white rounded-lg">
                      <p class="text-left text-h6 text-pink-darken-4">Discount <br> Months</p>
                      <div class="w-100 bg-pink-darken-4" style="height: 5px;"></div>
                      <p class="text-left text-h6 text-pink-darken-4">{{ totmonth }}</p>
                      <div class="w-100 bg-pink-darken-4" style="height: 5px;"></div>
                    </div>
                    <div class="w-50 pa-1 bg-white rounded-lg ">
                      <p class="text-right text-h6 text-pink-darken-4">Discount <br> Interest</p>
                      <div class="w-100 bg-pink-darken-4" style="height: 5px;"></div>
                      <p class="text-white text-h6 bg-pink-darken-4 mt-1">{{  }}</p>
                      <div class="w-100 bg-pink-darken-4 mt-1" style="height: 5px;"></div>
                    </div>

                  </div>
                  <div class="w-100 pa-1 d-flex justify-end ga-2">
                    <div class="w-25 pa-1 "></div>
                    <div class="w-25 pa-1 bg-white rounded-lg d-flex justify-center">
                      <p class="text-right text-h6 text-pink-darken-4">=</p>
                    </div>
                    <div class="w-50 pa-1 bg-white rounded-lg d-flex justify-center">
                      <p class="text-right text-h6 text-pink-darken-4">=</p>
                    </div>
                  </div>

                  <div class="w-100 pa-1 d-flex justify-end ga-2">
                    <div class="w-25 pa-1 "></div>
                    <div class="w-25 pa-1 bg-white rounded-lg d-flex justify-start">
                      <p class="text-left text-h6 text-pink-darken-4">Total <br> Months</p>
                    </div>
                    <div class="w-50 pa-1 bg-white rounded-lg d-flex justify-end">
                      <p class="text-right text-h6 text-pink-darken-4">Total <br> Interest</p>
                    </div>
                  </div>

                  <div class="w-100 pa-1 d-flex justify-end ga-2">
                    <div class="w-25 pa-1 "></div>
                    <div class="w-25 pa-1 bg-black rounded-lg d-flex justify-start">
                      <p class="text-left text-h6 text-white ">{{}}.00</p>
                    </div>
                    <div class="w-50 pa-1 bg-black rounded-lg d-flex justify-end">
                      <p class="text-right text-h6 text-white ">{{  }}</p>
                    </div>
                  </div>
    </v-tabs-window-item>
    <v-tabs-window-item value="four" class="pa-2">
     
   </v-tabs-window-item>
  </v-tabs-window>
  </v-card>
</v-bottom-sheet>
      
    </div>

  </div>
</template>
<script setup>
import { ref, onMounted, watch, onBeforeMount } from 'vue';
import CryptoJS from 'crypto-js';
import { useRoute, useRouter } from 'vue-router';

const content = ref(true);
const loading = ref(false);
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

const days=ref('')
const monthsval=ref('')
const daysval=ref('')
const total = ref('')
const roundmonths=ref('')
const advmonth=ref('')
const totmonth=ref('')
const perval=ref()
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



const dum=ref()
const calculation = (loandate) => {

  

  const loanamount = parseInt(loanvalue.value);
  const discountrate = discountRate.value;

  const loanDate = formatDater(loandate);
  const currentDate = formatDater(currentdate.value);
  const timeDifference = currentDate.getTime() - loanDate.getTime();
  const dayDifference = Math.ceil(timeDifference / (1000 * 60 * 60 * 24));
  const monthly_intrest = (loanamount * (discountrate / 100) / 12).toFixed(2)
  const no_of_month = Math.round(dayDifference / 30)

 perval.value=monthly_intrest

  const totalmonths = no_of_month + parseFloat(dum.value)
 

  const totalintrest = parseFloat((totalmonths * monthly_intrest).toFixed(2))
  const finalamount = loanamount + totalintrest
  total.value = finalamount
  interestamount.value = totalintrest
  days.value=dayDifference

  const months = Math.floor(dayDifference / 30);
  const dayss = dayDifference % 30;

  monthsval.value=months
  daysval.value=dayss
  roundmonths.value=totalmonths
 
getadvancemonth(roundmonths.value)


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

const getadvancemonth = async (roundval) => {
 
  const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/getadvancemonth.php';
 
 
  try {
    const response = await fetch(apiurl, { method: 'POST'});
    if (!response.ok) throw new Error('Failed to fetch response. Try again!');
    const data = await response.json();
    advmonth.value=data.AdvanceMonths
  const totalmonth=roundval-data.AdvanceMonths
  totmonth.value=totalmonth
dum.value=data.ExtraDays

  
  } catch (error) {
    errorpopup.value = true;
    errormessage.value = error.message;
  }
};
getadvancemonth()

const handlEvent=()=>{
  if(!discountRate.value){
    errorpopup.value = true;
    errormessage.value = 'Please set Discount Rate';
  }
  else if(!givenamount.value){
    errorpopup.value = true;
    errormessage.value = 'Please Enter Given amount ';
  }
  
  else{
    errorpopup.value = false;
    errormessage.value = '';
    calculationsubmission()
  }
}

const getFormattedDateTime = () => {
  const date = new Date();
  const formattedDate = date.toISOString().split('T')[0]; // dd-mm-yyyy
  const formattedTime = date.toLocaleTimeString('en-GB', { hour12: false }); // hh:mm:ss
  return `${formattedDate} ${formattedTime}`;
};

const currentDateTime = ref(getFormattedDateTime());



const calculationsubmission = async () => {
  const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/calculation.php';
  const formdata = new FormData();

  formdata.append('appid', userappid.value);
  // formdata.append('loanappid', );
  formdata.append('loannumber', loannumber.value,);
  formdata.append('loantype', loantype.value);
  formdata.append('loandate', loandate.value);
  formdata.append('loanvalue', loanvalue.value);
  formdata.append('discountRate', discountRate.value);
  formdata.append('givenamount', givenamount.value);
  formdata.append('interestamount', interestamount.value);
  formdata.append('offbook', offbook.value);
  formdata.append('total', total.value);
  formdata.append('in_amount', in_amount.value);
  formdata.append('datetime', currentDateTime.value);

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




//bottom sheet


</script>

<style></style>
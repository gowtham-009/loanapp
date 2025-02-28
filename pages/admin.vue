<template>
  <div v-if="isAuthenticated">
    <div v-if="errorpopup" style="position: absolute; width: 100%; z-index: 10;">
      <v-alert type="error">
        {{ errormessage }}
      </v-alert>
    </div>

    <div v-if="successpopup" style="position: absolute; width: 100%; z-index: 10;">
      <v-alert title="Success" type="success">
        {{ successmesage }}
      </v-alert>
    </div>
    <!--  -->
    <div class="pa-2 " :style="{ height: box1Height + 'px' }"  >
      <v-tabs v-model="tab" align-tabs="center"> 
        <v-tab value="saved">Saved</v-tab>
        <v-tab value="add">Add</v-tab>
        <v-tab value="onetime">One Time</v-tab>
      </v-tabs>

      <v-tabs-window v-model="tab">
        <v-tabs-window-item value="saved" class="pa-2 " >
          <v-form @submit.prevent="categoriesfilter_data">
            <div class="w-100 pa-1">
              <label for="" class="text-indigo font-weight-bold">Loan Type</label>
              <div class="options d-flex justify-center">
                <CheckboxButton v-for="option in options" :key="option.value" :name="'check-type1'"
                  :label="option.label" :value="option.value" v-model="selectedOption" class="pa-1" />
              </div>
            </div>

            <div class="w-100 pa-1 d-flex ga-2">
              <div class="w-100 pa-1">
                <label for="">Select Low Limit</label>
                <v-select v-model="selectedLowerLimit" label="Select Low Limit" :items="lowerlimit"
                  variant="solo-filled" :rules="[value => !value || value.length > 0 || 'Low limit is optional']" />
              </div>
              <div class="w-100 pa-1">
                <label for="">Select Upper Limit</label>
                <v-select v-model="selectedUpperLimit" label="Select Upper Limit" variant="solo-filled"
                  :items="upperlimit" :rules="[value => !value || value.length > 0 || 'Upper limit is optional']" />
              </div>
            </div>

            <v-btn class="text-white bg-indigo" type="submit" block>Search</v-btn>

          </v-form>

          <div class="d-flex justify-center h-screen d-flex justify-center align-center" v-if="loadingtab1">
            <v-progress-circular color="purple" indeterminate></v-progress-circular>
          </div>
          <div class="d-flex flex-column ga-2 mt-1 cont" >
           
            <p class="text-center">{{ nodata }}</p>
  <div v-for="data in resval" :key="data.id" class="pt-1 pa-1" >
    <div class="w-100 pa-1 d-flex flex-column  rounded bg-blue-grey-lighten-5">
      <div class="w-100 d-flex justify-space-between">
        <div class="text-indigo">ID: {{ data.id }}</div>
        <div class="text-indigo">{{ data.Loan_type1 }}</div>
      </div>
      <div>LowLimit: {{ data.LowLimit }}</div>
      <div>UpperLimit: {{ data.UpperLimit }}</div>
      <div>DiscountPeriod: {{ data.DiscountPeriod }}</div>
      <div>DiscountInterest: {{ data.DiscountInterest }}</div>
      <div>ElevatedInterest: {{ data.ElevatedInterest }}</div>
      <div class="w-100 pa-2 d-flex justify-space-between" style="gap: 10px;">
        <div class="w-100">
          <v-btn class="bg-red text-white" @click="edit_record(data.id)" block>Edit</v-btn>
        </div>
        <div class="w-100">
          <v-btn class="bg-blue text-white" @click="openDeleteDialog(data.id)" block>Delete</v-btn>
        </div>
      </div>
    </div>
  </div>
</div>

        </v-tabs-window-item>

        <v-tabs-window-item value="add">
          <p class="text-center text-indigo mt-1 font-weight-bold">Add Interest Record</p>
          <div class="d-flex justify-center h-screen d-flex justify-center align-center" v-if="loading">
            <v-progress-circular color="purple" indeterminate></v-progress-circular>
          </div>
          <v-form v-if="intrestform" @submit.prevent>
            <div class="w-100 d-flex justify-center flex-wrap">
              <CheckboxButton v-for="option in addoptions" :key="option.value" :name="'check-type2'"
                :label="option.label" :value="option.value" v-model="addselectedOption" class="pa-2" />
            </div>

            <div class="w-100 d-flex ga-2">

              <div class="w-100 pa-1">
                <label for="">Lower Limit</label>
                <v-text-field variant="solo-filled" v-model='lower_l' placeholder="0.00" type="number" hide-details
                  required class="w-100" :rules="lowerrules" @input="restrictDecimalPlaces_l"></v-text-field>
              </div>

              <div class="w-100 pa-1">
                <label for="">Upper Limit</label>
                <v-text-field v-model="upper_l" placeholder="0.00" type="number" hide-details required class="w-100"
                  :rules="upperrules" @input="restrictDecimalPlaces_u"></v-text-field>
              </div>

            </div>

            <div class="w-100 d-flex ga-2">

              <div class="w-100 pa-1">
                <label for="">Discount Period</label>
                <v-text-field v-model="discount" variant="solo-filled" placeholder="0.00" type="number" hide-details
                  required class="w-100" :rules="discountrules" @input="restrictDecimalPlaces_d"></v-text-field>
              </div>

              <div class="w-100 pa-1">
                <label for="">Discount Interest</label>
                <v-text-field variant="solo-filled" v-model="discount_interest" placeholder="0.00" type="number"
                  hide-details required class="w-100" :rules="discountinterest_r"
                  @input="restrictDecimalPlaces_di"></v-text-field>
              </div>

            </div>

            <div class="w-100 d-flex ga-2">

              <div class="w-100"></div>
              <div class="w-100">
                <label for="">Elevated Interest</label>
                <v-text-field variant="solo-filled" v-model="el_interest" type="number" hide-details required
                  class="w-100" :rules="elrules" @input="restrictDecimalPlaces_el"></v-text-field>
              </div>
                <input class="d-none" type="text" v-model="idval">
            </div>
            <div v-if="insertbtn" class="w-100 d-flex ga-3 pa-4 align-center">
              <div class="w-100"><v-btn class="bg-yellow text-black" @click="resetform()" block>Reset</v-btn></div>
              <div class="w-100"><v-btn class="bg-green text-white" @click="handleSubmit('insert')" type="submit" block>Insert</v-btn></div>
            </div>
            <div v-if="updatebtn" class="w-100 d-flex ga-3 pa-4 align-center">
              <div class="w-100"><v-btn class="bg-indigo text-white" @click="handleSubmit('update')" type="submit" block>Update</v-btn></div>
            </div>
          </v-form>
        </v-tabs-window-item>

        <v-tabs-window-item value="onetime">
          <v-btn block class="bg-warning" @click="toggleDisabled">Edit</v-btn>

          <v-form v-if="onetimeform" :class="{ 'disabled-box': isDisabled }" v-model="isValid" @submit.prevent>

            <div class="w-100">
              <p class="text-center text-indigo font-weight-bold">Onetime Setup</p>
              <div class="w-100 pa-1">
                <p>[1] Extra 3 Days after 1 Month will be calculated as ?</p>
                <div class="loan-2 w-100">

                  <div class="options d-flex justify-center flex-wrap">
                    <CheckboxButton v-for="option in optionsot" :key="option.value" :name="'check-type3'"
                      :label="option.label" :value="option.value" v-model="otselectedOption" class="pa-2" />
                  </div>
                </div>
              </div>
            </div>

            <hr class="mt-1">
            <div class="w-100 d-flex mt-2">
              <div class="w-100">
                <p>[2] How many months of Initial <br> Interest Deducation?(Type in Months)</p>
              </div>
              <div class="w-100">
                <v-text-field v-model="month" placeholder="1" type="number" hide-details required class="w-100"
                  :step="1" @keypress="blockInvalidChar"></v-text-field>
              </div>
            </div>

            <hr class="mt-1">
            <div class="w-100 mt-2">
              <p>[3] Do you need value handover calculation?</p>
              <div class="loan-2 w-100 d-flex  ga-10 justify-center align-center">
                <div class="options d-flex justify-center flex-wrap">
                  <CheckboxButton v-for="option in ysoptions" :key="option.value" :name="'check-type5'"
                    :label="option.label" :value="option.value" v-model="ysselectedOption" class="pa-2" />
                </div>
              </div>
            </div>

            <hr class="mt-1">
            <div class="w-100 mt-2">
              <p>[4] calculation Type?</p>
              <div class="loan-2 w-100 d-flex  justify-center">
                <div class="options d-flex justify-center flex-wrap">
                  <CheckboxButton v-for="option in caloptions" :key="option.value" :name="'check-type4'"
                    :label="option.label" :value="option.value" v-model="calselectedOption" class="pa-2" />
                </div>
              </div>
            </div>

            <hr class="mt-1">
            <div class="w-100 d-flex mt-2">
              <div class="w-100">
                <p>[5] Government Rate*</p>
              </div>
              <div class="w-100">
                <v-text-field v-model="govtrate" type="number" placeholder="1" hide-details required class="w-100"
                  :step="1" @keypress="blockInvalidChar"></v-text-field>
              </div>
            </div>

            <div class="w-100 mt-2 d-flex ga-2">
              <div class="w-100">
                <v-btn class="bg-indigo text-white" @click="handleSubmit3('update')" type="submit" block>Update</v-btn>
              </div>
              <div class="w-100">
                <v-btn class="bg-yellow text-black" @click="handleSubmit3('insert')" type="submit" block>Insert</v-btn>
              </div>
            </div>
          </v-form>

        </v-tabs-window-item>
      </v-tabs-window>


      <v-dialog
      v-model="dialog"
      width="auto"
    >
      <v-card
        max-width="400"
        prepend-icon="mdi-update"
        title="Are you sure delete this row?"
      >
       
        <div class="w-100 d-flex justify-end ga-2 pa-2">
          <v-btn
            class=" bg-red text-white"
            text="Delete"
           @click="delete_interest_id()"
          ></v-btn>
          <v-btn
           
            text="Close"
            @click="dialog = false"
          ></v-btn>
        </div>
     
      </v-card>
    </v-dialog> 

    </div>
  </div>
</template>

<script setup>
import { ref, onBeforeMount } from 'vue';
import CheckboxButton from '~/components/CheckboxButton.vue'; // Adjust the path as necessary
import { useRouter } from 'vue-router'
import { errorMessages } from 'vue/compiler-sfc';
const isAuthenticated = ref(false)
const loadingtab1=ref(false)
const errorpopup = ref(false)
const errormessage = ref(null)
const successmesage = ref(null)
const successpopup = ref(false)
onBeforeMount(() => {
  const token = localStorage.getItem('token')

  if (!token) {

    router.replace('/login')
  } else {

    isAuthenticated.value = true
  }
})

const tab = ref(null)
const deviceHeight = ref(0)
const box1Height = ref(0)

const updateSizes = () => {
  deviceHeight.value = window.innerHeight
  box1Height.value = deviceHeight.value * 0.99 // 8% height


}

onMounted(() => {
  updateSizes()
  window.addEventListener('resize', updateSizes) // Update sizes on window resize
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', updateSizes) // Clean up event listener
})

// tab-1 section

const options = ref([
  { label: 'ALL', value: 'all' },
  { label: 'Brass', value: 'brass' },
  { label: 'Gold', value: 'gold' },
  { label: 'Silver', value: 'silver' },
]);
const selectedOption = ref('');
const lowerlimit = ref([]);
const upperlimit = ref([]);
const selectedLowerLimit = ref(null);
const selectedUpperLimit = ref(null);

const dialog=ref(false)
const selectedId = ref(null)

const openDeleteDialog =(id)=>{
        selectedId.value = id
        dialog.value = true
      }

const getlimitsvalue = async () => {



  const apiurl_search = 'https://vaanam.w3webtechnologies.co.in/loandb/limitsvalue.php';

  try {
    const response_search = await fetch(apiurl_search, {
      method: 'GET',
    });

    if (!response_search.ok) {
      throw new Error('Failed to fetch limit values.');
    }

    const data_limits = await response_search.json();
    lowerlimit.value = data_limits.low1;
    upperlimit.value = data_limits.upper1;
  } catch (err) {
    errorpopup.value = true
    errormessage.value = err.message
  }
};

// Call the search function on component mount
getlimitsvalue();
const resval=ref([])
const nodata=ref('')
const categoriesfilter_data = async () => {
  loadingtab1.value = true;
  errormessage.value = null

  const formdatafilter = new FormData();
  formdatafilter.append('loan_type', selectedOption.value);

  if (selectedLowerLimit.value) {
    formdatafilter.append('lower_limit', selectedLowerLimit.value);
  }

  if (selectedUpperLimit.value) {
    formdatafilter.append('upper_limit', selectedUpperLimit.value);
  }

  const apiurlf = 'https://vaanam.w3webtechnologies.co.in/loandb/filter.php';
  try {
    const responsef = await fetch(apiurlf, {
      method: 'POST',
      body: formdatafilter,
    });

    if (!responsef.ok) {
      throw new Error('Failed to submit filter data.');
    }

    const datafilter = await responsef.json();
    if(datafilter.length>0){
      resval.value = datafilter; 
      nodata.value=''
    }
    else{
      nodata.value='Data is Empty'
    }
  
  } catch (err) {
    errorpopup.value=true
    errormessage.value=err.message
  } finally {
    loadingtab1.value = false;
  }
};



const delete_interest_id = async()=>{
       
        const formdata=new FormData();
        formdata.append('interestid',selectedId.value)
        const apiurl='https://vaanam.w3webtechnologies.co.in/loandb/interest_delete.php'
  
        try{
          const response= await fetch(apiurl,{
            method:'POST',
            body:formdata
          })
          if(!response.ok){
            throw new Error('Failed to submit filter data.');
          }
          else{
            const data=response.json()
          }
        
        }
        catch(err){
          errorpopup.value=true
          errormessage.value=error.message
          }
          finally{
            dialog.value = false 
            categoriesfilter_data()
          }
      }
  

const addselectedOption = ref('');
const lower_l = ref('')
const upper_l = ref('')
const discount_interest = ref('')
const discount = ref('')
const el_interest = ref('')
const idval=ref('')
const insertbtn=ref(true)
const updatebtn=ref(false)

const edit_record= async(rocored_id)=>{
  if(rocored_id){
    insertbtn.value=false
    updatebtn.value=true
    const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/interest_update.php';
    const formdata = new FormData();
    formdata.append('interestid', rocored_id);

    try {
      const response = await fetch(apiurl, {
          method: 'POST',
          body: formdata,
        });

        if (!response.ok) {
          throw new Error('Failed to submit filter data.');
        } else {
          const data = await response.json();
          
         
          addselectedOption.value = data[0]?.Loan_type1 || ''; 
          lower_l.value = data[0]?.LowLimit || '';
          upper_l.value = data[0]?.UpperLimit || ''; 
          discount.value = data[0]?.DiscountPeriod || ''; 
          discount_interest.value = data[0]?.DiscountInterest || ''; 
          el_interest.value = data[0]?.ElevatedInterest || '';
          idval.value=rocored_id
        }
    } catch (error) {
      errorpopup.value=true
      errormessage.value=error.message
    }
    finally{
      tab.value='add'
    }
  }


}


// tab-2 section
const addoptions = ref([
  { label: 'Gold', value: 'Gold' },
  { label: 'Silver', value: 'Silver' },
  { label: 'Brass', value: 'Brass' },
]);



const restrictDecimalPlaces_l = () => {
  let value = lower_l.value.replace(/[^0-9.]/g, ''); // Remove non-numeric characters except '.'
  let parts = value.split('.');

  if (parts.length > 2) {
    value = parts[0] + '.' + parts.slice(1).join('');
  }
  if (parts[1] && parts[1].length > 2) {
    value = parts[0] + '.' + parts[1].slice(0, 2);
  }

  lower_l.value = value;
};


const restrictDecimalPlaces_u = () => {
  let value = upper_l.value.replace(/[^0-9.]/g, ''); // Remove non-numeric characters except '.'
  let parts = value.split('.');

  if (parts.length > 2) {
    value = parts[0] + '.' + parts.slice(1).join('');
  }
  if (parts[1] && parts[1].length > 2) {
    value = parts[0] + '.' + parts[1].slice(0, 2);
  }

  upper_l.value = value;
};


const restrictDecimalPlaces_di = () => {
  let value = discount_interest.value.replace(/[^0-9.]/g, ''); // Remove non-numeric characters except '.'
  let parts = value.split('.');

  if (parts.length > 2) {
    value = parts[0] + '.' + parts.slice(1).join('');
  }
  if (parts[1] && parts[1].length > 2) {
    value = parts[0] + '.' + parts[1].slice(0, 2);
  }

  discount_interest.value = value;
};


const restrictDecimalPlaces_d = () => {
  let value = discount.value.replace(/[^0-9.]/g, ''); // Remove non-numeric characters except '.'
  let parts = value.split('.');

  if (parts.length > 2) {
    value = parts[0] + '.' + parts.slice(1).join('');
  }
  if (parts[1] && parts[1].length > 2) {
    value = parts[0] + '.' + parts[1].slice(0, 2);
  }

  discount.value = value;
};


const restrictDecimalPlaces_el = () => {
  let value = el_interest.value.replace(/[^0-9.]/g, ''); // Remove non-numeric characters except '.'
  let parts = value.split('.');

  if (parts.length > 2) {
    value = parts[0] + '.' + parts.slice(1).join('');
  }
  if (parts[1] && parts[1].length > 2) {
    value = parts[0] + '.' + parts[1].slice(0, 2);
  }

  el_interest.value = value;
};


const handleSubmit = async (datamode) => {
  if(datamode=='insert'){
      
  if (!addselectedOption.value) {
    errorpopup.value = true
    errormessage.value = 'loan type is Required'
  }
  else if (!lower_l.value) {
    errorpopup.value = true
    errormessage.value = 'lower value is Required'
  }
  else if (!upper_l.value) {
    errorpopup.value = true
    errormessage.value = 'upper value is Required'
  }
  else if (!discount.value) {
    errorpopup.value = true
    errormessage.value = 'dicount value is Required'
  }
  else if (!discount_interest.value) {
    errorpopup.value = true
    errormessage.value = 'discount intrest value is Required'
  }
  else if (!el_interest.value) {
    errorpopup.value = true
    errormessage.value = 'elevated intrest value is Required'
  }
  else {
    errorpopup.value = false
    errormessage.value = ''
    data_insert();
  }
  }


  if(datamode=='update'){
    
  if (!addselectedOption.value) {
    errorpopup.value = true
    errormessage.value = 'loan type is Required'
  }
  else if (!lower_l.value) {
    errorpopup.value = true
    errormessage.value = 'lower value is Required'
  }
  else if (!upper_l.value) {
    errorpopup.value = true
    errormessage.value = 'upper value is Required'
  }
  else if (!discount.value) {
    errorpopup.value = true
    errormessage.value = 'dicount value is Required'
  }
  else if (!discount_interest.value) {
    errorpopup.value = true
    errormessage.value = 'discount intrest value is Required'
  }
  else if (!el_interest.value) {
    errorpopup.value = true
    errormessage.value = 'elevated intrest value is Required'
  }
  else {
    errorpopup.value = false
    errormessage.value = ''
    update_form();
  }
  }



};



const loading = ref(false)
const intrestform = ref(true)
const getFormattedDateTime = () => {
  const date = new Date();
  const formattedDate = date.toISOString().split('T')[0]; // dd-mm-yyyy
  const formattedTime = date.toLocaleTimeString('en-GB', { hour12: false }); // hh:mm:ss
  return `${formattedDate} ${formattedTime}`;
};

const currentDateTime = ref(getFormattedDateTime());

const data_insert = async () => {

  loading.value = true
  intrestform.value = false
  const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/interest.php';
  const formData = new FormData();
  formData.append('datetime', currentDateTime.value);
  formData.append('Loan_type1', addselectedOption.value);
  formData.append('LowLimit', lower_l.value);
  formData.append('UpperLimit', upper_l.value);
  formData.append('DiscountPeriod', discount.value);
  formData.append('DiscountInterest', discount_interest.value);
  formData.append('ElevatedInterest', el_interest.value);

  try {
    const response = await fetch(apiurl, {
      method: 'POST',
      body: formData,
    });

    if (!response.ok) {
      throw new Error('Failed to submit data. Please check your inputs.');
    }

    const data = await response.json();
    // Handle the returned data as needed

  } catch (err) {
    errorpopup.value = true
    errormessage.value = err.message
  } finally {
    loading.value = false
    intrestform.value = true
    successpopup.value = true
    successmesage.value = 'Successfully Inserted'

    setTimeout(() => {
      successpopup.value = false
    }, 1000);
  }
};


const update_form = async()=>{
        loading.value=true
        intrestform.value=false
        const formdata=new FormData()
        formdata.append('datetime', currentDateTime.value);
        formdata.append('interestid', idval.value)
        formdata.append('Loan_type1', addselectedOption.value)
        formdata.append('LowLimit', lower_l.value)
        formdata.append('UpperLimit', upper_l.value)
        formdata.append('DiscountPeriod', discount.value)
        formdata.append('DiscountInterest',discount_interest.value)
        formdata.append('ElevatedInterest', el_interest.value)
        const apiurl='https://vaanam.w3webtechnologies.co.in/loandb/update_insert.php'
        try{
            const response=await fetch(apiurl,{
              method:'POST',
              body:formdata
            })
            if(!response.ok){
              throw new Error('Failed to submit filter data.');
            }
            else{
              const data=await response.json()

            }
        }
        catch(err){
          errorpopup.value=true
          errormessage.value=err.message
        }
        finally{
          loading.value=false
          intrestform.value=true
          successpopup.value = true
          successmesage.value = 'Successfully Updated'

    setTimeout(() => {
      successpopup.value = false
    }, 1000);
        }
    }


    const resetform=()=>{
      addselectedOption.value=''
      lower_l.value=''
      upper_l.value=''
      discount.value=''
      discount_interest.value=''
      el_interest.value=''
    }


// tab-3 section
const onetimeform = ref(true)
const isDisabled = ref(true);
const otselectedOption = ref('')
const month = ref('')
const ysselectedOption = ref('')
const calselectedOption = ref('')
const govtrate = ref('')

const toggleDisabled = () => {
  isDisabled.value = !isDisabled.value;
};



const optionsot = ref([
  { label: '0.5 Month', value: '0.5 Month' },
  { label: '1 Month', value: '1 Month' },
]);

const ysoptions = ref([
  { label: 'Yes', value: 'Yes' },
  { label: 'No', value: 'No' },
]);

const caloptions = ref([
  { label: 'Discount', value: 'Discount' },
  { label: 'Elevated', value: 'Elevated' },
]);

const blockInvalidChar = (event) => {
  // Prevent user from typing '.' or other non-numeric characters
  if (event.key === '.' || event.key === '-') {
    event.preventDefault();
  }
};


const setdata = async () => {

  const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/setting_get.php';
  try {
    const response = await fetch(apiurl, {
      method: 'GET',
    });
    if (!response.ok) {
      throw new Error('Failed to fetch data. Please check your inputs.');
    }
    const getdata = await response.json();
    otselectedOption.value = getdata[0].ExtraDays
    month.value = getdata[0].AdvanceMonths
    ysselectedOption.value = getdata[0].HandOver
    calselectedOption.value = getdata[0].CalculationMethod
    govtrate.value = getdata[0].GovtRate

  } catch (err) {
    errorpopup.value = true
    errormessage.value = err.message
  }
}
setdata()

const handleSubmit3 = async (typemode) => {
  if (typemode === 'update') {
    if (!otselectedOption.value) {
      errorpopup.value = true
      errormessage.value = 'Fild[1] is Required'
    }
    else if (!month.value) {
      errorpopup.value = true
      errormessage.value = 'Fild[2] is Required'
    }
    else if (!ysselectedOption.value) {
      errorpopup.value = true
      errormessage.value = 'Fild[3] value is Required'
    }
    else if (!calselectedOption.value) {
      errorpopup.value = true
      errormessage.value = 'Fild[4] value is Required'
    }
    else if (!govtrate.value) {
      errorpopup.value = true
      errormessage.value = 'Fild[5] value is Required'
    }
    else {
      errorpopup.value = false
      errormessage.value = ''
      updatedata()
    }
  }
  if (typemode === 'insert') {
    if (!otselectedOption.value) {
      errorpopup.value = true
      errormessage.value = 'Fild[1] is Required'
    }
    else if (!month.value) {
      errorpopup.value = true
      errormessage.value = 'Fild[2] is Required'
    }
    else if (!ysselectedOption.value) {
      errorpopup.value = true
      errormessage.value = 'Fild[3] value is Required'
    }
    else if (!calselectedOption.value) {
      errorpopup.value = true
      errormessage.value = 'Fild[4] value is Required'
    }
    else if (!govtrate.value) {
      errorpopup.value = true
      errormessage.value = 'Fild[5] value is Required'
    }
    else {
      errorpopup.value = false
      errormessage.value = ''
      settinginsertdata()
    }
  }
}



const updatedata = async () => {
  loading.value = true
  onetimeform.value = false
  const apiurl_u = 'https://vaanam.w3webtechnologies.co.in/loandb/setting.php';
  const formdata = new FormData();
  formdata.append('datetime', currentDateTime.value);
  formdata.append('ExtraDays', otselectedOption.value);
  formdata.append('AdvanceMonths', month.value);
  formdata.append('HandOver', ysselectedOption.value);
  formdata.append('CalculationMethod', calselectedOption.value);
  formdata.append('GovtRate', govtrate.value);

  try {
    const response = await fetch(apiurl_u, {
      method: 'POST',
      body: formdata,
    });

    if (!response.ok) {
      throw new Error('Failed to submit data. Please check your inputs.');
    }

    const data = await response.json();


  } catch (err) {
    errorpopup.value = true
    errormessage.value = err.message
  } finally {
    loading.value = false
    onetimeform.value = true
    successpopup.value = true
    successmesage.value = 'Successfully Updated'

    setTimeout(() => {
      successpopup.value = false
    }, 1000);
    isDisabled.value = true
  }

}

const settinginsertdata = async () => {
  loading.value = true
  onetimeform.value = false
  const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/settinginsert.php';
  const formdata = new FormData();
  formdata.append('datetime', currentDateTime.value);
  formdata.append('ExtraDays', otselectedOption.value);
  formdata.append('AdvanceMonths', month.value);
  formdata.append('HandOver', ysselectedOption.value);
  formdata.append('CalculationMethod', calselectedOption.value);
  formdata.append('GovtRate', govtrate.value);

  try {
    const response = await fetch(apiurl, {
      method: 'POST',
      body: formdata,
    });

    if (!response.ok) {
      throw new Error('Failed to submit data. Please check your inputs.');
    }

    const data = await response.json();
    data.value = data

  } catch (err) {
    errorpopup.value = true
    errormessage.value = err.message
  } finally {
    loading.value = false
    onetimeform.value = true
    successpopup.value = true
    successmesage.value = 'Successfully Inserted'

    setTimeout(() => {
      successpopup.value = false
    }, 1000);
  }
  isDisabled.value = true
}

</script>

<style>
.disabled-box {
  pointer-events: none;
  /* Disable pointer events */
  opacity: 0.5;
  /* Optional: make it look visually disabled */
}

.cont {
  flex-grow: 1;
  overflow-y: auto;
  min-height: 300px; /* Set a reasonable minimum height */
  max-height: calc(100vh - 150px); /* Adjust based on header/footer heights */
}

</style>
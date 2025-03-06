<template>
    <div v-if="isAuthenticated" class="main-container d-flex justify-space-between flex-column w-100 scroller"
        :style="{ height: deviceHeight + 'px' }">
        <div class="w-100  bg-blue" :style="{ height: boxex1Height + 'px' }"></div>
        <div class="w-100 scroller" :style="{ height: boxex2Height + 'px' }">

            <div class="w-100 pa-2 ">
                <v-form @submit.prevent="balance_check">
                    <v-row class="pa-1" no-gutters>


                        <v-col cols="12">
                            <input v-model="formattedDate"   type="date" class="w-100  rounded-lg text-center pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>

                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col cols="5">
                            <input readonly v-model="previousdate" type="text"
                                class="w-100  rounded-lg text-center pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col cols="2"></v-col>
                        <v-col cols="5">
                            <p class="text-center rounded-lg text-h6" style="border: 1px solid black; padding: 4px;">
                                Balance:{{ previousamount }}</p>
                        </v-col>

                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                            <input readonly v-model="num1" type="number" class="w-100  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input type="number" v-model="notes1" class="w-100 text-left  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input readonly type="number" :value="row1" class="w-100  rounded-lg pl-1 pa-2 text-right"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                            <input readonly v-model="num2" type="number" class="w-100  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input type="number" v-model="notes2" class="w-100 text-left  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input readonly type="number" :value="row2" class="w-100 text-right  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                            <input readonly v-model="num3" type="number" class="w-100  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input type="number" v-model="notes3" class="w-100 text-left  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input readonly type="number" :value="row3" class="w-100 text-right  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                            <input readonly v-model="num4" type="number" class="w-100  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input type="number" v-model="notes4" class="w-100 text-left  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input readonly type="number" :value="row4" class="w-100 text-right  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                            <input readonly v-model="num5" type="number" class="w-100  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input type="number" v-model="notes5" class="w-100 text-left  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input readonly type="number" :value="row5" class="w-100 text-right  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                            <input readonly v-model="num6" type="number" class="w-100  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input type="number" v-model="notes6" class="w-100 text-left  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input readonly type="number" :value="row6" class="w-100 text-right  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                            <input readonly v-model="num7" type="number" class="w-100  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input type="number" v-model="notes7" class="w-100 text-left  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input readonly type="number" :value="row7" class="w-100 text-right  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                            <input readonly v-model="num8" type="number" class="w-100  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input type="number" v-model="notes8" class="w-100 text-left  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input readonly type="number" :value="row8" class="w-100 text-right  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                            <input readonly v-model="num9" type="number" class="w-100  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input type="number" v-model="notes9" class="w-100  rounded-lg pl-1 pa-2 text-left"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input readonly type="number" :value="row9" class="w-100 text-right  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                            <input readonly v-model="num10" type="number" class="w-100  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1  d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input type="number" v-model="notes10" class="w-100  rounded-lg pl-1 pa-2 text-left"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col class="pa-1" cols="4">
                            <input readonly type="number" :value="row10" class="w-100 text-right  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>
                    </v-row>
                    <v-row class="pa-1" no-gutters>
                        <v-col class="pa-1" cols="2">
                        </v-col>
                        <v-col class="pa-1 d-flex justify-end" cols="3">
                            <p class="text-h5">Total</p>
                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">

                        </v-col>
                        <v-col class="pa-1" cols="6">
                            <input readonly type="number" :value="total" class="w-100 text-right  rounded-lg pl-1 pa-2"
                                style="outline: none; border: 1px solid black;  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);">
                        </v-col>


                    </v-row>
                    <v-row no-gutters>
                        <v-col cols="2"></v-col>
                        <v-col cols="10">
                            <v-btn class="pa-2 bg-blue text-white" size="large" type="submit" block>Submit</v-btn>
                        </v-col>
                    </v-row>
                </v-form>
            </div>
        </div>
        <div class="w-100  bg-blue" :style="{ height: boxex3Height + 'px' }"></div>
    </div>

</template>


<script setup>
import { ref, onMounted,watch, onBeforeUnmount, computed } from 'vue';
import { useRouter } from 'vue-router'


const router = useRouter()
const isAuthenticated = ref(false)

onBeforeMount(() => {
    const token = localStorage.getItem('token')

    if (!token) {

        router.replace('/login')
    } else {

        isAuthenticated.value = true
    }
})
const alertmsg = ref(false)
// Reactive state
const deviceWidth = ref(0);
const deviceHeight = ref(0);
const boxex1Height = ref(0);
const boxex2Height = ref(0);
const boxex3Height = ref(0);

const currentdate = new Date();
  const formattedDate = ref(`${currentdate.getFullYear()}-${String(currentdate.getMonth() + 1).padStart(2, '0')}-${String(currentdate.getDate()).padStart(2, '0')}`);
  
currentdate.setDate(currentdate.getDate() - 1);
// Values for denominations
const num1 = 2000;
const num2 = 500;
const num3 = 200;
const num4 = 100;
const num5 = 50;
const num6 = 20;
const num7 = 10;
const num8 = 5;
const num9 = 2;
const num10 = 1;

// Reactive note counts and state
const notes1 = ref(0);
const notes2 = ref(0);
const notes3 = ref(0);
const notes4 = ref(0);
const notes5 = ref(0);
const notes6 = ref(0);
const notes7 = ref(0);
const notes8 = ref(0);
const notes9 = ref(0);
const notes10 = ref(0);
const loading = ref(true);
const error = ref(null);


const previousamount = ref("0")
const previousdate = ref('')
// Update device dimensions
const updateDeviceDimensions = () => {
    deviceWidth.value = window.innerWidth;
    deviceHeight.value = window.innerHeight;

    // Calculate heights for each box based on percentages
    boxex1Height.value = Math.floor(deviceHeight.value * 0.05);
    boxex2Height.value = Math.floor(deviceHeight.value * 0.90);
    boxex3Height.value = Math.floor(deviceHeight.value * 0.05);
};

// Computed row totals
const row1 = computed(() => num1 * notes1.value);
const row2 = computed(() => num2 * notes2.value);
const row3 = computed(() => num3 * notes3.value);
const row4 = computed(() => num4 * notes4.value);
const row5 = computed(() => num5 * notes5.value);
const row6 = computed(() => num6 * notes6.value);
const row7 = computed(() => num7 * notes7.value);
const row8 = computed(() => num8 * notes8.value);
const row9 = computed(() => num9 * notes9.value);
const row10 = computed(() => num10 * notes10.value);

// Total computed property
const total = computed(() => {
    return row1.value + row2.value + row3.value + row4.value + row5.value +
        row6.value + row7.value + row8.value + row9.value + row10.value;
});

// Lifecycle hooks
onMounted(() => {
    updateDeviceDimensions(); // Initial call to set dimensions on component mount
    window.addEventListener('resize', updateDeviceDimensions); // Listen for window resize events
});

onBeforeUnmount(() => {
    window.removeEventListener('resize', updateDeviceDimensions); // Clean up: remove resize event listener
});


const getFormattedDateTime = () => {
    const date = new Date();
    const formattedDate = date.toISOString().split('T')[0]; // dd-mm-yyyy
    const formattedTime = date.toLocaleTimeString('en-GB', { hour12: false }); // hh:mm:ss
    return `${formattedDate} ${formattedTime}`;
};

const currentDateTime = ref(getFormattedDateTime());


const balance_check = async () => {
    loading.value = true;
    error.value = null;
    const balancecheck_api = 'http://vaanam.w3webtechnologies.co.in/loandb/balance.php';
    const formdata = new FormData();
    formdata.append('date', formattedDate.value);
    formdata.append('thousand_2', notes1.value);
    formdata.append('hundred_5', notes2.value);
    formdata.append('hundred_2', notes3.value);
    formdata.append('hundred_1', notes4.value);
    formdata.append('fifty_50', notes5.value);
    formdata.append('twenty_20', notes6.value);
    formdata.append('ten_10', notes7.value);
    formdata.append('five_5', notes8.value);
    formdata.append('two_2', notes9.value);
    formdata.append('one_1', notes10.value);
    formdata.append('day_total', total.value);
    formdata.append('datetime', currentDateTime.value);

    try {
        const response = await fetch(balancecheck_api, {
            method: "POST",
            body: formdata
        });
        if (!response.ok) {
            throw new Error('Failed to fetch limit values.');
        } else {
            const data = await response.json();
            console.log(data); // Handle response data as needed
        }
    } catch (err) {
        error.value = err.message;
    } finally {
        loading.value = false;
        alertmsg.value = true

    }
};


const previousbalance = async () => {
    loading.value = true;
    error.value = null;
    const apiurl = 'http://vaanam.w3webtechnologies.co.in/loandb/previous_balance.php';
    try {
        const response = await fetch(apiurl, {
            method: "POST",

        });
        if (!response.ok) {
            throw new Error('Failed to fetch previous balance values.');
        } else {
            const data = await response.json();
            previousamount.value = data.data.DayTotal
            let rawDate = data.data.Date; // Assuming this is in a standard format like "YYYY-MM-DD"
            let dateObj = new Date(rawDate);

            let formattedDate = dateObj.getDate().toString().padStart(2, '0') + '-' +
                (dateObj.getMonth() + 1).toString().padStart(2, '0') + '-' +
                dateObj.getFullYear();
            previousdate.value = formattedDate;
        }
    } catch (err) {
        error.value = err.message;
    }
}

previousbalance()


const get_today_datevalue=async()=>{
   
   
    const apiurl = 'http://vaanam.w3webtechnologies.co.in/loandb/get_balancerow.php';
    const formdata = new FormData();
    formdata.append('dateval', formattedDate.value);
    try {
        const response = await fetch(apiurl, {
            method: "POST",
            body: formdata

        });
        if (!response.ok) {
            throw new Error('Failed to fetch previous balance values.');
        } else {
            const data = await response.json();
            notes1.value=data.data.t_thousand ||'0'
            notes2.value=data.data.f_hundred ||'0'
            notes3.value=data.data.t_hundred ||'0'
            notes4.value=data.data.o_hundred ||'0'
            notes5.value=data.data.f_fifty ||'0'
            notes6.value=data.data.t_twenty ||'0'
            notes7.value=data.data.t_ten ||'0'
            notes8.value=data.data.f_five ||'0'
            notes9.value=data.data.t_two ||'0'
            notes10.value=data.data.o_one ||'0'
          
         
        }
    } catch (err) {
        error.value = err.message;
    }
}



    onMounted(() => {
        get_today_datevalue()
    });

    watch(formattedDate, () => {
        get_today_datevalue()
    });


</script>
<style>
html,
body {
    overflow-y: hidden;

}

.scroller {
    overflow: hidden;
    overflow-y: scroll;
}
</style>
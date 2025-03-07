<template>
    <div v-if="isAuthenticated" class="main-container d-flex justify-space-between flex-column w-100 scroller"
        :style="{ height: deviceHeight + 'px' }">
        <div class="w-100  bg-blue" :style="{ height: boxex1Height + 'px' }"></div>
        <div class="w-100 scroller" :style="{ height: boxex2Height + 'px' }">

         
            <div class="w-100 pa-2 ">
                <v-form @submit.prevent="balance_check">
                    <v-row class="pa-1" no-gutters>

                        <v-col cols="12"> 
                            <input type="date" class="w-100 text-center rounded mb-2"  v-model="formattedDate" style="height: 50px; border: 1px solid gray;">
                        </v-col>

                    </v-row>
                    <v-row  no-gutters>
                        <v-col cols="5">
                            <v-text-field label="Previous Date" class="pa-0 "  readonly v-model="previousdate" type="text" variant="solo"></v-text-field>
                        </v-col>
                        <v-col cols="2"></v-col>
                        <v-col cols="5">
                            <v-text-field class="pa-0 text-center" label="Balance"  readonly v-model="previousamount" type="text" variant="solo"></v-text-field>
                        </v-col>

                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                            <v-text-field class="pa-0 text-center"   readonly v-model="num1" type="text" variant="solo"></v-text-field>

                           
                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 text-center" v-model="notes1" type="text" variant="solo"></v-text-field>
                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 right-center"   readonly v-model="row1" type="text" variant="solo"></v-text-field>
                        </v-col>
                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                            <v-text-field class="pa-0 text-center"   readonly v-model="num2" type="text" variant="solo"></v-text-field>
                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 text-center" v-model="notes2" type="text" variant="solo"></v-text-field>

                        </v-col>
                        <v-col class="pa-1 d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 right-center"   readonly v-model="row2" type="text" variant="solo"></v-text-field>

                           
                        </v-col>
                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                            <v-text-field class="pa-0 text-center"   readonly v-model="num3" type="text" variant="solo"></v-text-field>
                        </v-col>
                        <v-col class="d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 text-center" v-model="notes3" type="text" variant="solo"></v-text-field>

                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 right-center"   readonly v-model="row3" type="text" variant="solo"></v-text-field>

                        </v-col>
                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                            <v-text-field class="pa-0 text-center"   readonly v-model="num4" type="text" variant="solo"></v-text-field>
                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 text-center" v-model="notes4" type="text" variant="solo"></v-text-field>

                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 right-center"   readonly v-model="row4" type="text" variant="solo"></v-text-field>

                        </v-col>
                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                            <v-text-field class="pa-0 text-center"   readonly v-model="num5" type="text" variant="solo"></v-text-field>
                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 text-center" v-model="notes5" type="text" variant="solo"></v-text-field>

                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 right-center"   readonly v-model="row5" type="text" variant="solo"></v-text-field>

                        </v-col>
                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                            <v-text-field class="pa-0 text-center"   readonly v-model="num6" type="text" variant="solo"></v-text-field>
                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 text-center" v-model="notes6" type="text" variant="solo"></v-text-field>

                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 right-center"   readonly v-model="row6" type="text" variant="solo"></v-text-field>

                        </v-col>
                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                            <v-text-field class="pa-0 text-center"   readonly v-model="num7" type="text" variant="solo"></v-text-field>
                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 text-center" v-model="notes7" type="text" variant="solo"></v-text-field>

                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 right-center"   readonly v-model="row7" type="text" variant="solo"></v-text-field>

                        </v-col>
                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                            <v-text-field class="pa-0 text-center"   readonly v-model="num8" type="text" variant="solo"></v-text-field>
                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 text-center" v-model="notes8" type="text" variant="solo"></v-text-field>

                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 right-center"   readonly v-model="row8" type="text" variant="solo"></v-text-field>

                        </v-col>
                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                            <v-text-field class="pa-0 text-center"   readonly v-model="num9" type="text" variant="solo"></v-text-field>
                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 text-center" v-model="notes9" type="text" variant="solo"></v-text-field>

                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 right-center"   readonly v-model="row9" type="text" variant="solo"></v-text-field>

                        </v-col>
                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                            <v-text-field class="pa-0 text-center"   readonly v-model="num10" type="text" variant="solo"></v-text-field>

                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">
                            <b>X</b>
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 text-center" v-model="notes10" type="text" variant="solo"></v-text-field>

                        </v-col>
                        <v-col class="d-flex justify-center align-center" cols="1">
                            =
                        </v-col>
                        <v-col  cols="4">
                            <v-text-field class="pa-0 right-center"   readonly v-model="row10" type="text" variant="solo"></v-text-field>

                        </v-col>
                    </v-row>
                    <v-row  no-gutters>
                        <v-col  cols="2">
                        </v-col>
                        <v-col class=" d-flex justify-end align-center" cols="3">
                            <p class="text-h5 text-grey">Total</p>
                        </v-col>
                        <v-col class=" d-flex justify-center align-center" cols="1">

                        </v-col>
                        <v-col  cols="6">
                            <v-text-field class="pa-0 right-center"   readonly :value="total" type="text" variant="solo"></v-text-field>

                           
                        </v-col>


                    </v-row>
                    <v-row no-gutters>
                        <v-col cols="2"></v-col>
                        <v-col cols="10">
                            <v-btn class="pa-2 bg-green text-white" size="large" type="submit" block>Submit</v-btn>
                        </v-col>
                    </v-row>
                </v-form>
            </div>
        </div>
        <div class="w-100  bg-blue" :style="{ height: boxex3Height + 'px' }"></div>
    </div>

</template>


<script setup>
import { ref, onMounted, watch, onBeforeUnmount, computed } from 'vue';
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
const num1 = 2000.00
const num2 = 500.00;
const num3 = 200.00;
const num4 = 100.00;
const num5 = 50.00;
const num6 = 20.00;
const num7 = 10.00;
const num8 = 5.00;
const num9 = 2.00;
const num10 = 1.00;

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
const row1 = computed(() => (num1 * notes1.value).toFixed(2));
const row2 = computed(() => (num2 * notes2.value).toFixed(2));
const row3 = computed(() => (num3 * notes3.value).toFixed(2));
const row4 = computed(() => (num4 * notes4.value).toFixed(2));
const row5 = computed(() => (num5 * notes5.value).toFixed(2));
const row6 = computed(() => (num6 * notes6.value).toFixed(2));
const row7 = computed(() => (num7 * notes7.value).toFixed(2));
const row8 = computed(() => (num8 * notes8.value).toFixed(2));
const row9 = computed(() => (num9 * notes9.value).toFixed(2));
const row10 = computed(() => (num10 * notes10.value).toFixed(2));

// Total computed property
const total = computed(() => {
    return (
        parseFloat(row1.value) +
        parseFloat(row2.value) +
        parseFloat(row3.value) +
        parseFloat(row4.value) +
        parseFloat(row5.value) +
        parseFloat(row6.value) +
        parseFloat(row7.value) +
        parseFloat(row8.value) +
        parseFloat(row9.value) +
        parseFloat(row10.value)
    ).toFixed(2); // Ensures 2 decimal places
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
    const balancecheck_api = 'https://vaanam.w3webtechnologies.co.in/loandb/balance.php';
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

        }
    } catch (err) {
        error.value = err.message;
    } finally {
        loading.value = false;
        alertmsg.value = true

    }
};





const get_today_datevalue = async () => {


    const apiurl = 'https://vaanam.w3webtechnologies.co.in/loandb/get_balancerow.php';
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
            notes1.value = data.t_thousand || '0'
            notes2.value = data.f_hundred || '0'
            notes3.value = data.t_hundred || '0'
            notes4.value = data.o_hundred || '0'
            notes5.value = data.f_fifty || '0'
            notes6.value = data.t_twenty || '0'
            notes7.value = data.t_ten || '0'
            notes8.value = data.f_five || '0'
            notes9.value = data.t_two || '0'
            notes10.value = data.o_one || '0'

            let previoustot=parseFloat(data.prevDayTotal)
            previousamount.value = previoustot.toFixed(2)
            let date = new Date(formattedDate.value);
            date.setDate(date.getDate() - 1);


            let previousDate = date.toLocaleDateString('en-GB').split('/').join('-'); // Format as dd-mm-yyyy

            previousdate.value = previousDate; // Assign formatted date


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

.right-center input {
    text-align: right;
}
</style>
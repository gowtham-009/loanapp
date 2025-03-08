<template>
  <div>
  <div class="w-100  bg-blue" :style="{ height: boxex1Height + 'px' }"></div>
   <div class="w-100 scroller" :style="{ height: boxex2Height + 'px' }">
      <div class="w-100" :style="{ height: tabHeight + 'px' }" style="border: 1px solid red;">

      </div>
      <div class="w-100 scroller" :style="{ height: cardHeight + 'px' }" style="border: 1px solid black;">
        <v-card
   
    prepend-icon="$vuetify"
    subtitle="The #1 Vue UI Library"
   class="w-100"
  >
    <template v-slot:title>
      <span class="font-weight-black">Welcome to Vuetify</span>
    </template>

    <v-card-text class="bg-surface-light pt-4">
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus! Eaque cupiditate minima, at placeat totam, magni doloremque veniam neque porro libero rerum unde voluptatem!
    </v-card-text>
  </v-card>

  <v-card
   
   prepend-icon="$vuetify"
   subtitle="The #1 Vue UI Library"
  class="w-100"
 >
   <template v-slot:title>
     <span class="font-weight-black">Welcome to Vuetify</span>
   </template>

   <v-card-text class="bg-surface-light pt-4">
     Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus! Eaque cupiditate minima, at placeat totam, magni doloremque veniam neque porro libero rerum unde voluptatem!
   </v-card-text>
 </v-card>

 <v-card
   
   prepend-icon="$vuetify"
   subtitle="The #1 Vue UI Library"
  class="w-100"
 >
   <template v-slot:title>
     <span class="font-weight-black">Welcome to Vuetify</span>
   </template>

   <v-card-text class="bg-surface-light pt-4">
     Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus! Eaque cupiditate minima, at placeat totam, magni doloremque veniam neque porro libero rerum unde voluptatem!
   </v-card-text>
 </v-card>

 <v-card
   
   prepend-icon="$vuetify"
   subtitle="The #1 Vue UI Library"
  class="w-100"
 >
   <template v-slot:title>
     <span class="font-weight-black">Welcome to Vuetify</span>
   </template>

   <v-card-text class="bg-surface-light pt-4">
     Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus! Eaque cupiditate minima, at placeat totam, magni doloremque veniam neque porro libero rerum unde voluptatem!
   </v-card-text>
 </v-card>


 <v-card
   
   prepend-icon="$vuetify"
   subtitle="The #1 Vue UI Library"
  class="w-100"
 >
   <template v-slot:title>
     <span class="font-weight-black">Welcome to Vuetify</span>
   </template>

   <v-card-text class="bg-surface-light pt-4">
     Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus! Eaque cupiditate minima, at placeat totam, magni doloremque veniam neque porro libero rerum unde voluptatem!
   </v-card-text>
 </v-card>


 <v-card
   
   prepend-icon="$vuetify"
   subtitle="The #1 Vue UI Library"
  class="w-100"
 >
   <template v-slot:title>
     <span class="font-weight-black">Welcome to Vuetify</span>
   </template>

   <v-card-text class="bg-surface-light pt-4">
     Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus! Eaque cupiditate minima, at placeat totam, magni doloremque veniam neque porro libero rerum unde voluptatem!
   </v-card-text>
 </v-card>

 <v-card
   
   prepend-icon="$vuetify"
   subtitle="The #1 Vue UI Library"
  class="w-100"
 >
   <template v-slot:title>
     <span class="font-weight-black">Welcome to Vuetify</span>
   </template>

   <v-card-text class="bg-surface-light pt-4">
     Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus! Eaque cupiditate minima, at placeat totam, magni doloremque veniam neque porro libero rerum unde voluptatem!
   </v-card-text>
 </v-card>

 <v-card
   
   prepend-icon="$vuetify"
   subtitle="The #1 Vue UI Library"
  class="w-100"
 >
   <template v-slot:title>
     <span class="font-weight-black">Welcome to Vuetify</span>
   </template>

   <v-card-text class="bg-surface-light pt-4">
     Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus! Eaque cupiditate minima, at placeat totam, magni doloremque veniam neque porro libero rerum unde voluptatem!
   </v-card-text>
 </v-card>

 <v-card
   
   prepend-icon="$vuetify"
   subtitle="The #1 Vue UI Library"
  class="w-100"
 >
   <template v-slot:title>
     <span class="font-weight-black">Welcome to Vuetify</span>
   </template>

   <v-card-text class="bg-surface-light pt-4">
     Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus! Eaque cupiditate minima, at placeat totam, magni doloremque veniam neque porro libero rerum unde voluptatem!
   </v-card-text>
 </v-card>
</div>
   </div>
   <div class="w-100 bg-blue" :style="{ height: boxex3Height + 'px' }"></div>

  </div>
</template>

<script setup>

import { ref, onMounted, watch, onBeforeUnmount, computed } from 'vue';
const deviceWidth = ref(0);
const deviceHeight = ref(0);
const boxex1Height = ref(0);
const boxex2Height = ref(0);
const boxex3Height = ref(0);
const tabHeight=ref(0);
const cardHeight=ref(0)

const updateDeviceDimensions = () => {
    deviceWidth.value = window.innerWidth;
    deviceHeight.value = window.innerHeight;

    // Calculate heights for each box based on percentages
    boxex1Height.value = Math.floor(deviceHeight.value * 0.05);
    boxex2Height.value = Math.floor(deviceHeight.value * 0.90);
    boxex3Height.value = Math.floor(deviceHeight.value * 0.05);
    tabHeight.value= Math.floor(  boxex2Height.value * 0.40);
    cardHeight.value=Math.floor(  boxex2Height.value * 0.60);
};

onMounted(() => {
    updateDeviceDimensions(); // Initial call to set dimensions on component mount
    window.addEventListener('resize', updateDeviceDimensions); // Listen for window resize events
});

onBeforeUnmount(() => {
    window.removeEventListener('resize', updateDeviceDimensions); // Clean up: remove resize event listener
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
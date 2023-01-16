<template>
     <div class="bg-gray-100 h-screen p-20">
          <div class="bg-white border border-gray-300 overflow-hidden h-[calc(100vh-150px)] rounded-xl p-5">
               <filterComponent
                    :boarding="fromStoppages"
                    :dropping="toStoppages"
                    @filterTripList="tripDataRender"
               />
               <div class="mt-5">
                    <triplist :trips="tripData"/>
               </div>
          </div>
     </div>
</template>

<script setup>
     import { onBeforeMount, ref } from 'vue';
     import availabletrips from '@/assets/json/trip.json';
     import triplist from '@/components/tripList/index.vue';
     import filterComponent from '@/components/filter/index.vue'

     const fromStoppages = ref({})
     const toStoppages = ref({})
     const tripData = ref(availabletrips)
     const filteredTable = ref({})

     const tripDataRender = ((filterData)=>{
          filteredTable.value = availabletrips.filter((item)=>{
               let result = true 
               filterData.fromStoppage && (result &&= item.fromStoppage === filterData.fromStoppage)
               filterData.toStoppage && (result &&= item.toStoppage === filterData.toStoppage)
               filterData.tripdate && (result &&= item.tripDate === filterData.tripdate)
               return result
          })
          tripData.value = filteredTable.value
     })

     fromStoppages.value = tripData.value.map(item => {
          return item.fromStoppage
     })
     toStoppages.value = tripData.value.map(item => {
          return item.toStoppage
     })
</script>

<style scoped>

</style>
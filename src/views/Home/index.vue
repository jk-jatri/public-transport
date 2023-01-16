<template>
     <div class="bg-gray-100 h-screen p-20">
          <div class="bg-white border border-gray-300 overflow-y-auto h-[calc(100vh-150px)] rounded-xl p-5">
               <filterComponent
                    :boarding="fromStoppages"
                    :dropping="toStoppages"
                    @filterTripList="tripListRender"
                    @sortList="sortTripList"
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

     const tripListRender = ((filterData)=>{
          filteredTable.value = availabletrips.filter((item)=>{
               let result = true 
               filterData.fromStoppage && (result &&= item.fromStoppage === filterData.fromStoppage)
               filterData.toStoppage && (result &&= item.toStoppage === filterData.toStoppage)
               filterData.tripdate && (result &&= item.tripDate === filterData.tripdate)
               filterData.vehicleType && (result &&= item.vehicleType.toLowerCase() === filterData.vehicleType.toLowerCase())
               return result
          })
          tripData.value = filteredTable.value
     })

     const sortTripList = ((order)=>{
          if(order === 'asc') {
               tripData.value.sort((a,b) => (a.fare > b.fare) ? 1 : ((b.fare > a.fare) ? -1 : 0));
          } else {
               tripData.value.sort((a,b) => (a.fare < b.fare) ? 1 : ((b.fare < a.fare) ? -1 : 0));
          }
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
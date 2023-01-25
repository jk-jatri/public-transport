<template>
     <div class="bg-gray-100 h-screen p-20">
          <div class="bg-white border border-gray-300 h-[calc(100vh-150px)] rounded-xl p-5">
               <filterComponent
                    :boarding="fromStoppages"
                    :dropping="toStoppages"
                    @filterTripList="tripListRender"
                    @sortList="sortTripList"
               />
               <triplist :trips="tripData" :sortOrder = "sortingOrder"/>
          </div>
     </div>
</template>

<script setup>
     import { ref } from 'vue';
     import availabletrips from '@/assets/json/trip.json';
     import triplist from '@/components/tripList/index.vue';
     import filterComponent from '@/components/filter/index.vue'
     const sortingOrder = ref('')
     const fromStoppages = ref({})
     const toStoppages = ref({})
     let tripData = ref(availabletrips)
     const filteredTable = ref({})

     const tripListRender = ((filterValue)=>{
          filteredTable.value = availabletrips.filter((item)=>{
               let result = true 
               filterValue.fromStoppage && (result &&= item.fromStoppage === filterValue.fromStoppage)
               filterValue.toStoppage && (result &&= item.toStoppage === filterValue.toStoppage)
               filterValue.tripDate && (result &&= item.tripDate === filterValue.tripDate)
               filterValue.vehicleType && (result &&= item.vehicleType.toLowerCase() === filterValue.vehicleType.toLowerCase())
               return result
          })
          tripData.value = filteredTable.value
     })



     const sortTripList = ((order)=>{
          sortingOrder.value = order
          if(order === 'dsc') {
               tripData.value.sort((a,b) => (a.fare < b.fare) ? 1 : ((b.fare < a.fare) ? -1 : 0));
          } else {
               tripData.value.sort((a,b) => (a.fare > b.fare) ? 1 : ((b.fare > a.fare) ? -1 : 0));
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
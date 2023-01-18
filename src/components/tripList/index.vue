<template>
     <div class="mt-5 overflow-y-auto h-[calc(100%-250px)]" @scroll="handleScroll">
          <div v-if="trips.length<=0" class="text-center h-full flex flex-col items-center justify-center">
               <p class="animate-pulse">There is no available trips</p>
          </div>
          <table v-else class="border-collapse border border-slate-600 table-auto w-full">
               <thead class="sticky top-0">
                    <tr class="bg-gray-500">
                         <th class="border border-slate-600 text-white">SL.</th>
                         <th class="border border-slate-600 text-white">Trip Code</th>
                         <th class="border border-slate-600 text-white">Trip Date & Time</th>
                         <th class="border border-slate-600 text-white">Route</th>
                         <th class="border border-slate-600 text-white">Type</th>
                         <th class="border border-slate-600 text-white">Fare</th>
                         <th class="border border-slate-600 text-white">Direction</th>
                         <th class="border border-slate-600 text-white">Status</th>
                         <th class="border border-slate-600 text-white">Action</th>
                    </tr>
               </thead>
               <tbody v-for="(item, index) in allTrips" :key="item._id">
                    <tr class="h-10" :class="(index+1)%2 === 0 ? 'bg-slate-200': 'bg-slate-100'">
                         <td class="border border-slate-600 text-center">{{ index+1 }}</td>
                         <td class="border border-slate-600 text-center">{{item.tripCode}}</td>
                         <td class="border border-slate-600 text-center">{{item.tripDate}}</td>
                         <td class="border border-slate-600 text-center">{{item.fromStoppage}} - {{ item.toStoppage}}</td>
                         <td class="border border-slate-600 text-center">{{item.vehicleType}}</td>
                         <td class="border border-slate-600 text-center">{{item.fare}}</td>
                         <td class="border border-slate-600 text-center">{{item.direction}}</td>
                         <td class="border border-slate-600 text-center">
                              <div class="w-1/2 mx-auto text-white px-1 py-1 rounded-md text-sm" :class="item.status ? 'bg-green-700': 'bg-red-700'">
                                   {{item.status ? 'Active' : 'Inactive'}}
                              </div>
                         </td>
                         <td class="border border-slate-600 text-center font-bold">
                              <button class="border w-1/2 text-gray-300 rounded-md bg-gray-800" v-if="tripActive !== item._id" @click="viewDetails(item)">View</button>
                              <button class="border w-1/2 bg-gray-300 rounded-md text-gray-800" v-else @click="viewDetails('')">Close</button>
                         </td>
                    </tr>
                    <tr v-if="tripActive === item._id">
                         <td colspan="9" class="p-3">
                              <div class="rounded-lg border border-slate-400 p-5">
                                   <table class="border-collapse border border-slate-300 mx-auto table-auto w-1/3">
                                        <tr class="odd:bg-slate-50 even:bg-slate-100 h-10">
                                             <th class="border border-slate-300 text-left w-[200px] pl-4">Trip Id</th> 
                                             <td class="border border-slate-300 text-center">{{tripDetails._id}}</td>
                                        </tr>
                                        <tr class="odd:bg-slate-50 even:bg-slate-100 h-10">
                                             <th class="border border-slate-300 text-left w-[200px] pl-4">Trip Code</th> 
                                             <td class="border border-slate-300 text-center">{{tripDetails.tripCode}}</td>
                                        </tr>
                                        <tr class="odd:bg-slate-50 even:bg-slate-100 h-10">
                                             <th class="border border-slate-300 text-left w-[200px] pl-4">From Stoppage</th> 
                                             <td class="border border-slate-300 text-center">{{tripDetails.fromStoppage}}</td>
                                        </tr>
                                        <tr class="odd:bg-slate-50 even:bg-slate-100 h-10">
                                             <th class="border border-slate-300 text-left w-[200px] pl-4">To Stoppage</th> 
                                             <td class="border border-slate-300 text-center">{{tripDetails.toStoppage}}</td>
                                        </tr>
                                        <tr class="odd:bg-slate-50 even:bg-slate-100 h-10">
                                             <th class="border border-slate-300 text-left w-[200px] pl-4">Trip date</th> 
                                             <td class="border border-slate-300 text-center">{{tripDetails.tripDate}}</td>
                                        </tr>
                                        <tr class="odd:bg-slate-50 even:bg-slate-100 h-10">
                                             <th class="border border-slate-300 text-left w-[200px] pl-4">Vehicle Type</th> 
                                             <td class="border border-slate-300 text-center">{{tripDetails.vehicleType}}</td>
                                        </tr>
                                        <tr class="odd:bg-slate-50 even:bg-slate-100 h-10">
                                             <th class="border border-slate-300 text-left w-[200px] pl-4">Trip Fare</th> 
                                             <td class="border border-slate-300 text-center">{{tripDetails.fare}}</td>
                                        </tr>
                                        <tr class="odd:bg-slate-50 even:bg-slate-100 h-10">
                                             <th class="border border-slate-300 text-left w-[200px] pl-4">Trip Direction</th> 
                                             <td class="border border-slate-300 text-center">{{tripDetails.direction}}</td>
                                        </tr>
                                        <tr class="odd:bg-slate-50 even:bg-slate-100 h-10">
                                             <th class="border border-slate-300 text-left w-[200px] pl-4">Trip Status</th> 
                                             <td class="border border-slate-300 text-center">
                                                  <div class="w-1/2 mx-auto text-white px-1 py-1 rounded-md text-sm" :class="tripDetails.status ? 'bg-green-700': 'bg-red-700'">
                                                       {{tripDetails.status ? 'Active' : 'Inactive'}}
                                                  </div>
                                             </td>
                                        </tr>
                                   </table>
                              </div>
                         </td>
                    </tr>
               </tbody>
          </table>
          
     </div>
</template>

<script setup>
     import { onBeforeMount, ref, watch } from 'vue';

     const props = defineProps({
          trips: {type: Object},
          sortOrder: {type: String}
     })
     const tripActive = ref('')
     const tripDetails = ref({})
     let tripData = ref({})
     let allTrips = ref([]);



     let perPage = ref(15);
     let loadedData = ref(15);
     let respondedData = ref(15);

     const viewDetails = (tripInfo) =>{
          tripActive.value = tripInfo._id
          tripDetails.value = tripInfo
     }
     
          watch(
               () => props.trips,
               () => {
                    tripData.value = props.trips
                    allTrips.value = props.trips.slice(0, perPage.value);
               }
          )

     watch(
          () => props.sortOrder,
          () => {
               if(props.sortOrder === 'dsc') {
                    allTrips.value.sort((a,b) => (a.fare < b.fare) ? 1 : ((b.fare < a.fare) ? -1 : 0));
               } else {
                    allTrips.value.sort((a,b) => (a.fare > b.fare) ? 1 : ((b.fare > a.fare) ? -1 : 0));
               }
          }
     )
          
     allTrips.value = props.trips.slice(0, perPage.value);

     const handleScroll = ((e)=> {
          const { scrollTop, offsetHeight, scrollHeight } = e.target
          if ((scrollTop + offsetHeight) >= scrollHeight) {
               if (perPage.value <= respondedData.value) {
                    loadMoreTrips();
               }
          }
     });
     const loadMoreTrips = (()=> {
          let newArray = props.trips.slice(loadedData.value, loadedData.value+perPage.value)
          loadedData.value += newArray.length
          respondedData.value = newArray.length
          allTrips.value = allTrips.value.concat(newArray)
     })
</script>

<style scoped>

</style>
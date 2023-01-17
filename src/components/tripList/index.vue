<template>
     <table class="border-collapse border border-slate-500 table-auto w-full">
          <thead>
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
          <tbody v-for="(item, index) in trips" :key="item._id">
               <tr :class="(index+1)%2 === 0 ? 'bg-slate-200': 'bg-slate-100'">
                    <td class="border border-slate-600 text-center">{{ index+1 }}</td>
                    <td class="border border-slate-600 text-center">{{item.tripCode}}</td>
                    <td class="border border-slate-600 text-center">{{item.tripDate}}</td>
                    <td class="border border-slate-600 text-center">{{item.fromStoppage}} - {{ item.toStoppage}}</td>
                    <td class="border border-slate-600 text-center">{{item.vehicleType}}</td>
                    <td class="border border-slate-600 text-center">{{item.fare}}</td>
                    <td class="border border-slate-600 text-center">{{item.direction}}</td>
                    <td class="border border-slate-600 text-center">
                         <span class="text-white px-1 rounded-md text-sm" :class="item.status ? 'bg-green-700': 'bg-red-700'">
                              {{item.status ? 'Active' : 'Inactive'}}
                         </span>
                    </td>
                    <td class="border border-slate-600 text-center font-bold">
                         <button v-if="tripActive !== item._id" @click="viewDetails(item)">view</button>
                         <button v-else @click="viewDetails('')">close</button>
                    </td>
               </tr>
               <tr v-if="tripActive === item._id">
                    <td colspan="9" class="p-3">
                         <div class="rounded-lg border border-slate-400 p-5">
                              <table class="border-collapse border border-slate-300 mx-auto table-auto w-1/2">
                                   <tr class="odd:bg-slate-50 even:bg-slate-100">
                                        <th class="border border-slate-300 text-center">Trip Id</th> 
                                        <td class="border border-slate-300 text-center">{{tripDetails._id}}</td>
                                   </tr>
                                   <tr class="odd:bg-slate-50 even:bg-slate-100">
                                        <th class="border border-slate-300 text-center">Trip Code</th> 
                                        <td class="border border-slate-300 text-center">{{tripDetails.tripCode}}</td>
                                   </tr>
                                   <tr class="odd:bg-slate-50 even:bg-slate-100">
                                        <th class="border border-slate-300 text-center">From Stoppage</th> 
                                        <td class="border border-slate-300 text-center">{{tripDetails.fromStoppage}}</td>
                                   </tr>
                                   <tr class="odd:bg-slate-50 even:bg-slate-100">
                                        <th class="border border-slate-300 text-center">To Stoppage</th> 
                                        <td class="border border-slate-300 text-center">{{tripDetails.toStoppage}}</td>
                                   </tr>
                                   <tr class="odd:bg-slate-50 even:bg-slate-100">
                                        <th class="border border-slate-300 text-center">Trip date</th> 
                                        <td class="border border-slate-300 text-center">{{tripDetails.tripDate}}</td>
                                   </tr>
                                   <tr class="odd:bg-slate-50 even:bg-slate-100">
                                        <th class="border border-slate-300 text-center">Vehicle Type</th> 
                                        <td class="border border-slate-300 text-center">{{tripDetails.vehicleType}}</td>
                                   </tr>
                                   <tr class="odd:bg-slate-50 even:bg-slate-100">
                                        <th class="border border-slate-300 text-center">Trip Fare</th> 
                                        <td class="border border-slate-300 text-center">{{tripDetails.fare}}</td>
                                   </tr>
                                   <tr class="odd:bg-slate-50 even:bg-slate-100">
                                        <th class="border border-slate-300 text-center">Trip Direction</th> 
                                        <td class="border border-slate-300 text-center">{{tripDetails.direction}}</td>
                                   </tr>
                                   <tr class="odd:bg-slate-50 even:bg-slate-100">
                                        <th class="border border-slate-300 text-center">Trip Status</th> 
                                        <td class="border border-slate-300 text-center">
                                             <span class="text-white px-1 rounded-md text-sm" :class="tripDetails.status ? 'bg-green-700': 'bg-red-700'">
                                                  {{tripDetails.status ? 'Active' : 'Inactive'}}
                                             </span>
                                        </td>
                                   </tr>
                              </table>
                         </div>
                    </td>
               </tr>
          </tbody>
     </table>
</template>

<script setup>
import { ref } from 'vue';

const tripActive = ref('')
const tripDetails = ref({})
const props = defineProps({
     trips: {type: Object},
})

const viewDetails = (tripInfo) =>{
     tripActive.value = tripInfo._id
     tripDetails.value = tripInfo
}
</script>

<style scoped>

</style>
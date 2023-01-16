<template>
     <table class="border-collapse border border-slate-500 table-auto w-full">
          <thead>
               <tr>
                    <th class="border border-slate-600">SL.</th>
                    <th class="border border-slate-600">Trip Code</th>
                    <th class="border border-slate-600">Trip Date & Time</th>
                    <th class="border border-slate-600">Route</th>
                    <th class="border border-slate-600">Fare</th>
                    <th class="border border-slate-600">Status</th>
                    <th class="border border-slate-600">Action</th>
               </tr>
          </thead>
          <tbody>
               <tr v-for="(item, index) in trips" :key="item._id">
                    <td class="border border-slate-600 text-center">{{ index+1 }}</td>
                    <td class="border border-slate-600 text-center">{{item.tripCode}}</td>
                    <td class="border border-slate-600 text-center">{{item.tripDate}}</td>
                    <td class="border border-slate-600 text-center">{{item.fromStoppage}} - {{ item.toStoppage}}</td>
                    <td class="border border-slate-600 text-center">{{item.fare}}</td>
                    <td class="border border-slate-600 text-center">{{item.direction}}</td>
                    <td class="border border-slate-600 text-center"><button>view</button></td>
               </tr>
          </tbody>
     </table>
</template>

<script setup>
const props = defineProps({
     trips: {type: Object},
})
</script>

<style scoped>

</style>
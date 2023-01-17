<template>
     <div class="rounded-lg bg-gray-100 border border-gray-300 p-3">
          <div class="divide-y divide-gray-400">
               <form @submit.prevent="submitForm(filterData)" class="flex justify-start gap-x-3 pb-3">
                    <div class="w-3/4">
                         <div class="flex justify-between gap-x-3">
                              <dropdown
                                   :id="'fromStoppage'"
                                   :label="'Select From Stoppage'"
                                   v-model="filterData.fromStoppage"
                                   :options="props.boarding"
                                   :defaultOption="'Select From Stoppage'"
                              />
                              <dropdown
                                   :id="'toStoppage'"
                                   :label="'Select To Stoppage'"
                                   v-model="filterData.toStoppage"
                                   :options="props.dropping"
                                   :defaultOption="'Select To Stoppage'"
                              />
                              <date
                                   :id="'tripdate'"
                                   :label="'Select Trip Date'"
                                   v-model="modifyFilterData.tripdate"
                              />
                         </div>
                    </div>
                    <div class="w-1/4">
                         <button type="submit" class="bg-gray-400 text-white w-full h-full rounded-lg border-gray-300">Submit</button>
                    </div>
               </form>
               <div class="flex justify-start gap-x-3 pt-3">
                    <div class="w-3/4 flex justify-between gap-x-3">
                         <dayWiseFilter
                              :id="'dayWiseFilter'"
                              :label="'Day Wise Filter'"
                              v-model="modifyFilterData.tripdate"
                         />
                         <vehicleTypeFilter
                              :id="'selectType'"
                              :label="'Select Vehicle Type'"
                              v-model="modifyFilterData.vehicleType"
                         />
                         <priceFilter
                              :id="'sortBy'"
                              :label="'Sort By Price'"
                              v-model="modifyFilterData.sortBy"
                              :defaultOption="'Sort By Order'"
                         />
                         
                    </div>
                    <div class="w-1/4"></div>
               </div>
          </div>
          <p>Showing Result For: <b>{{ filterData.tripdate}}</b></p>
     </div>
</template>

<script setup>
     import dropdown from '@/components/filter/dropdown.vue';
     import date from '@/components/filter/date.vue';
     import dayWiseFilter from '@/components/filter/modifyFilter/dateFilter.vue';
     import vehicleTypeFilter from '@/components/filter/modifyFilter/vehicleTypeFilter.vue';
     import priceFilter from '@/components/filter/modifyFilter/priceFilter.vue';
     import { ref, watch } from 'vue';

     const props = defineProps({
          boarding: {type: Object},
          dropping: {type: Object}
     })

     const filterData = ref({
          fromStoppage : "",
          toStoppage : "",
          tripdate : new Date().toISOString().split('T')[0],
          vehicleType: "ALl",
          sortBy: ""
     })

     const modifyFilterData = ref({
          tripdate: new Date().toISOString().split('T')[0],
          vehicleType: "All",
          sortBy: ""
     })

     const emit = defineEmits(['filterTripList', 'sortList'])

     const submitForm = ((data)=> {
          emit("filterTripList", data);
     })

     watch(
          () => modifyFilterData.value.tripdate,
          () => {
               filterData.value.tripdate = modifyFilterData.value.tripdate
               emit("filterTripList", modifyFilterData.value);
          }
     )

     watch(
          () => modifyFilterData.value.vehicleType,
          () => {
               console.log("modifyFilterData.value.vehicleType");
               emit("filterTripList", modifyFilterData.value);
          }
     )

     watch(
          () => modifyFilterData.value.sortBy,
          () => {
               emit("sortList", modifyFilterData.value.sortBy);
          }
     )

</script>

<style scoped>

</style>
<template>
     <div class="h-[230px] rounded-lg bg-gray-100 border border-gray-300 p-3">
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
                                   :id="'tripDate'"
                                   :label="'Select Trip Date'"
                                   v-model="modifyFilterData.tripDate"
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
                              v-model="modifyFilterData.tripDate"
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
          <p>Showing Result For: <b>{{ filterData.tripDate}}</b></p>
     </div>
</template>

<script setup>
     import dropdown from '@/components/filter/dropdown.vue';
     import date from '@/components/filter/date.vue';
     import dayWiseFilter from '@/components/filter/modifyFilter/dateFilter.vue';
     import vehicleTypeFilter from '@/components/filter/modifyFilter/vehicleTypeFilter.vue';
     import priceFilter from '@/components/filter/modifyFilter/priceFilter.vue';
     import { onBeforeMount, onMounted, ref, watch } from 'vue';

     const props = defineProps({
          boarding: {type: Object},
          dropping: {type: Object}
     })

     const filterData = ref({
          fromStoppage : "",
          toStoppage : "",
          tripDate : new Date().toISOString().split('T')[0],
          vehicleType: null,
          sortBy: ""
     })

     const modifyFilterData = ref({
          tripDate: new Date().toISOString().split('T')[0],
          vehicleType: null,
          sortBy: ""
     })

     const emit = defineEmits(['filterTripList', 'sortList'])

     const submitForm = ((data)=> {
          emit("filterTripList", data);
     })

     onBeforeMount(()=>{
          submitForm(filterData.value);
     })

     watch(
          () => modifyFilterData.value.tripDate,
          () => {
               filterData.value.tripDate = modifyFilterData.value.tripDate
               emit("filterTripList", modifyFilterData.value);
          }
     )

     watch(
          () => modifyFilterData.value.vehicleType,
          () => {
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
<template>
     <div class="rounded-lg bg-gray-100 border border-gray-300 p-3">
          <form @submit.prevent="submitForm(filterData)" class="flex justify-start gap-x-3 divide-x">
               <div class="w-3/4 divide-y">
                    <div class="flex justify-between gap-x-3 p-2">
                         <dropdown
                              :id="'fromStoppage'"
                              :label="'From Stoppage'"
                              v-model="filterData.fromStoppage"
                              :options="props.boarding"
                              :defaultOption="'-select From Stoppage-'"
                         />
                         <dropdown
                              :id="'toStoppage'"
                              :label="'To Stoppage'"
                              v-model="filterData.toStoppage"
                              :options="props.dropping"
                              :defaultOption="'-select To Stoppage-'"
                         />
                         <date
                              :id="'tripdate'"
                              :label="'Trip Date'"
                              v-model="filterData.tripdate"
                         />
                    </div>
                    <div class="flex justify-start gap-x-3 p-2">
                         <div>

                         </div>
                    </div>
               </div>
               <div class="p-2 w-1/4">
                    <button type="submit" class="bg-gray-400 text-white w-full h-full rounded-lg border-gray-300">Submit</button>
               </div>
          </form>
     </div>
</template>

<script setup>
     import dropdown from '@/components/filter/dropdown.vue';
     import date from '@/components/filter/date.vue';
     import { ref, watch } from 'vue';

     const props = defineProps({
          boarding: {type: Object},
          dropping: {type: Object}
     })

     const filterData = ref({
          fromStoppage : "",
          toStoppage : "",
          tripdate : ""
     })

     const emit = defineEmits(['filterTripList'])

     const submitForm = ((data)=> {
          emit("filterTripList", data);
     })

</script>

<style scoped>

</style>
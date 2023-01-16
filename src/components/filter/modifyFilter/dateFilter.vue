<template>
     <div class="flex flex-col gap-y-1 bg-white border p-3 rounded-lg w-1/3">
          <label :for="props.id">{{label}}</label>
          <div class="flex justify-between gap-x-2">
               <button @click="previousDayTrip" class="bg-gray-100 border w-1/3">Prev</button>
               <button @click="todaysTrip" class="bg-gray-100 border w-1/3">Today</button>
               <button @click="nextDayTrip" class="bg-gray-100 border w-1/3">Next</button>
          </div>
     </div>
</template>

<script setup>
     import { ref, watch } from 'vue';

     const props = defineProps({
          id: {type: String},
          label: {type: String},
          modelValue: {type: String},
     })
     const emit = defineEmits(['update:modelValue'])
     const tripDate = ref(props.modelValue)

     watch(
          () => tripDate.value,
          () => {
               emit("update:modelValue", tripDate.value);
          }
     );

     const previousDayTrip = () => {
          let previousDate = new Date(tripDate.value)
          previousDate.setDate(previousDate.getDate()-1)
          tripDate.value = previousDate.toISOString().split('T')[0]
          console.log("previousDate =>", tripDate.value);
     }
     
     const todaysTrip = () => {
          let today = new Date().toISOString().split('T')[0]
          tripDate.value = today
          console.log("today =>", tripDate.value);
     }
     
     const nextDayTrip = () => {
          let nextDate = new Date(tripDate.value)
          nextDate.setDate(nextDate.getDate()+1)
          tripDate.value = nextDate.toISOString().split('T')[0]
          console.log("nextDate =>", tripDate.value);
     }

</script>

<style scoped>

</style>
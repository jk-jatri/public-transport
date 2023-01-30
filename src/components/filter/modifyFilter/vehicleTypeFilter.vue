<template>
     <div class="flex flex-col gap-y-1 bg-white border p-3 rounded-lg w-1/3">
          <label :for="props.id" class="font-semibold">{{label}}</label>
          <div class="flex justify-start gap-x-2">
               <div v-for="(type, index) in vehicleTypes" :key="index"> 
                    <input 
                         :id="type" type="checkbox" 
                         @click="filterByVehicleType(checked ? type : '')" 
                         :checked="vehicleType === type"
                    >
                    <label :for="type">{{type}}</label>
               </div>
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
     const vehicleTypes = ref(["Ac" , "Non-Ac"]);

     const emit = defineEmits(['update:modelValue'])
     const vehicleType = ref(props.modelValue)
     
     const filterByVehicleType = (type) =>{
          console.log("type=>", type);
          vehicleType.value = type
          emit("update:modelValue", vehicleType.value)
     }
     watch(
          () => vehicleType.value,
          () => {
               emit("update:modelValue", vehicleType.value);
          }
     )
</script>

<style scoped>

</style>
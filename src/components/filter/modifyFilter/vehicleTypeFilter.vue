<template>
     <div class="flex flex-col gap-y-1 bg-white border p-3 rounded-lg w-1/3">
          <label :for="props.id">{{label}}</label>
          <div class="flex justify-start gap-x-2">
               <div v-for="(item, index) in vehicleTypes" :key="index"> 
                    <input 
                         :id="item" type="checkbox" 
                         @click="vehicleType = item" 
                         :checked="vehicleType === item"
                    >
                    <label :for="item">{{item}}</label>
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
     const vehicleTypes = ref(["ac" , "non-ac"]);

     const emit = defineEmits(['update:modelValue'])
     const vehicleType = ref(props.modelValue)

     watch(
          () => vehicleType.value,
          () => {
               emit("update:modelValue", vehicleType.value);
          }
     );
</script>

<style scoped>

</style>
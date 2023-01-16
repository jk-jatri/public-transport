<template>
     <div class="flex flex-col gap-y-1 bg-white border p-3 rounded-lg w-1/3">
          <label :for="props.id">{{label}}</label>
          <select :id="props.id" v-model="sortBy">
               <option value="">{{ defaultOption }}</option>
               <option v-for="(type, index) in sortTypes" :key="index" :value="type">{{type}}</option>
          </select>
     </div>
</template>

<script setup>
     import { ref, watch } from 'vue';

     const props = defineProps({
          id: {type: String},
          label: {type: String},
          modelValue: {type: String},
          defaultOption: {type: String},
     })

     const sortTypes = ref(["asc" , "dsc"]);

     const emit = defineEmits(['update:modelValue'])
     const sortBy = ref(props.modelValue)

     watch(
          () => sortBy.value,
          () => {
               emit("update:modelValue", sortBy.value);
          }
     );
</script>

<style scoped>

</style>
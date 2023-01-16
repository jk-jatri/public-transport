<template>
     <div class="flex flex-col gap-y-1 bg-white border p-3 rounded-lg w-1/3">
          <label :for="props.id">{{label}}</label>
          <input :id="props.id" type="date" v-model="tripDate">
     </div>
</template>

<script setup>
     import { ref, watch } from 'vue';

     const props = defineProps({
          id: {type: String},
          label: {type: String},
          modelValue: {type: String},
     })
     const tripDate = ref(props.modelValue)
     const emit = defineEmits(['update:modelValue'])

     watch(
          () => tripDate.value,
          () => {
               emit("update:modelValue", tripDate.value);
          }
     );
</script>

<style scoped>

</style>
<template>
     <div class="flex flex-col gap-y-1 bg-white border p-3 rounded-lg w-1/3">
          <label :for="props.id">{{label}}</label>
          <select :id="props.id" v-model="stoppage">
               <option value="">{{ defaultOption }}</option>
               <option v-for="(option, index) in stoppages" :key="index" :value="option">{{option}}</option>
          </select>
     </div>
</template>

<script setup>
     import { onMounted, ref, watch } from 'vue';
     const props = defineProps({
          id: {type: String},
          label: {type: String},
          options: {type: Object},
          defaultOption: {type: String},
          modelValue: {type: String},
     })

     const stoppages = ref({})
     const stoppage = ref(props.modelValue)
     onMounted(()=>{
          stoppages.value = Array.from(new Set(props.options))
     })
     const emit = defineEmits(['update:modelValue'])

     watch(
          () => stoppage.value,
          () => {
          emit("update:modelValue", stoppage.value);
          }
     );
</script>

<style scoped>

</style>
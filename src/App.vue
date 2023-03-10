<script setup lang="ts">
import { ref } from 'vue';
import CustomInput from './components/CustomInput.vue';

const value = ref("test");
</script>

<template>
  {{ value }}
  <CustomInput v-model="value" />
</template>



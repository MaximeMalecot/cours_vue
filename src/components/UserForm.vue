<script setup>
import { reactive } from "vue";
import UserInput from "./UserInput.vue";
const formData = reactive({
  identifier: "test",
  pwd: "",
});
</script>

<template>
  <form @submit.prevent="$emit('submit', formData)">
    <UserInput
      v-model:identifier="formData.identifier"
      v-model:pwd="formData.pwd"
    />
    <button type="submit">Submit</button>
  </form>
</template>

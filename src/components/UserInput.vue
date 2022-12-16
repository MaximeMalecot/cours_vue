<script setup>
defineProps(["identifier", "pwd"]);
</script>

<template>
  <div>
    <input
      type="text"
      :value="identifier"
      @input="$emit('update:identifier', $event.target.value)"
    />
    <input
      type="text"
      :value="pwd"
      @input="$emit('update:pwd', $event.target.value)"
    />
  </div>
</template>

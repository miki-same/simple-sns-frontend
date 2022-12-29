<script setup lang="ts">
  const props = defineProps({
    item:String,
    placeholder:String
  })
</script>

<template>
  <input type="text" :placeholder=props.placeholder class="input input-bordered w-full max-w-xs"
    :v-model=props.item />
</template>
<script setup lang="ts">
  const props = defineProps<{
    username: string
    message: string
    posted_at: string
  }>()
</script>

<template>
<div class="chat chat-start">
  <div class="chat-header">
    {{ props.username }}
    <time class="text-xs opacity-50">{{ props.posted_at }}</time>
  </div>
  <div class="chat-bubble">{{ props.message }}</div>
</div>
</template>
<script setup lang="ts">
  const props = defineProps<{
    username: string
    message: string
    posted_at: string
  }>()
</script>

<template>
<div class="card w-full bg-base-100 shadow-xl mx-auto mb-3">
  <div class="card-body">
    <h2 class="card-title">{{ props.username }}</h2>
    <p class="text-lg">{{ props.message }}</p>
    <p>{{ props.posted_at }}</p>
    <div class="card-actions justify-end">
      <button class="btn btn-primary">返信</button>
      <label class="swap swap-rotate text-5xl">

        <!-- this hidden checkbox controls the state -->
        <input type="checkbox" />
        <div class="swap-on">💖</div>
        <div class="swap-off">🤍</div>
      </label>

    </div>
  </div>
</div>
</template>
<script setup lang="ts">
  const emit=defineEmits(['logout']);
</script>
<template>
  <!-- The button to open modal -->
  <label for="login-btn" class="btn flex-none">ログアウト</label>
  <!-- Put this part before </body> tag -->
  <input type="checkbox" id="login-btn" class="modal-toggle" />
  <div class="modal">
    <div class="modal-box">
      <h4 class="normal-case text-2xl font-midium">ログアウトしますか？</h4>
      <button class="btn btn-primary w-full max-w-xs" @click="emit('logout')">ログアウト</button>
      <div class="modal-action">
        <label for="login-btn" class="btn">閉じる</label>
      </div>
    </div>
  </div>
</template>
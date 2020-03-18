<template>
    <div class="loading">
      <div class="loading-spinner"></div>
    </div>
</template>

<script>
export default {
    name: 'Loading'
}
</script>

<style>
@keyframes spin {
  to { transform: rotate(360deg); }
}
.loading {
  position: absolute;
  z-index: 9999;
  width: 100%;
  height: 100%;
  display: flex;
  background-color: rgba(0, 0, 0, 0.8);
  justify-content: center;
  align-items: center;
}
.loading-spinner {
  border: 8px solid rgb(255, 255, 255);
  border-radius: 50%;
  border-left-color: lightcoral;
  width: 50px;
  height: 50px;
  animation: spin .5s linear infinite;
}
</style>
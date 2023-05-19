<template>
    <div id="main-container">
      <slot />
  </div>
</template>

<style scoped>
#main-container {
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
  gap: 20px;
  width: 100%;
  max-height: 100vh;
  padding: 30px;
  overflow-y: auto;
}
</style>
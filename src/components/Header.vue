<template>
  <div class="header">
    <img src="/img/logo.png" alt="Logo" class="logo">
    <h2>{{ this.title }}</h2>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: {
    title: {
      type: String,
      default: 'Infrastructure Status'
    },
  },
}
</script>

<style scoped>
.header {
  display: flex;
  align-items: center;
}
.logo {
  width: 60px;
  height: auto;
}
h2 {
  margin: 0;
  margin-left: 20px;
}
</style>
<template>
  <div class="flex ai-center">
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
.logo {
  width: 60px;
  height: auto;
}
h2 {
  margin: 0;
  margin-left: 20px;
}
@media screen and (max-width: 768px) {
  .logo {
    width: 40px;
  }
  h2 {
    font-size: 1.2rem;
  }
}
</style>
<template>
  <div class="flex flex-row jc-space-between history">
    <div v-for="(result, index) in preparedResults" :key="index" :title="result.timestamp" :class="{blob: true, green: result.success === true, orange: result.success === false, grey: !result.timestamp}"></div>
  </div>
</template>

<script>
export default {
  name: 'EndpointHistory',
  props: {
    results: {
      type: Array,
      required: true
    }
  },
  computed: {
    preparedResults() {
      let tmp = [...this.results];
      while (tmp.length < 20) {
        tmp.unshift({});
      }
      for (let t of tmp) {
        t.timestamp = new Date(t.timestamp).toLocaleString();
      }
      return tmp;
    }
  }
}
</script>

<style scoped>
.history {
 width: 300px;
}
.blob {
  width: 7px;
  height: 20px;
  margin: 0;
  border-radius: 50rem;
  --hover-scale: 1.3;
}
.blob:hover {
  transform: scale(var(--hover-scale));
  transition-duration: 100ms;
}
.green {
  background-color: var(--green);
}
.orange {
  background-color: var(--orange);
}
.grey {
  background-color: var(--grey);
}
@media screen and (max-width: 768px) {
  .history {
    width: 100%;
    margin-top: 0.4rem
  }
}
</style>
<template>
  <div class="history">
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
      return tmp;
    }
  }
}
</script>

<style scoped>
.history {
 display: flex;
 flex-direction: row;
 justify-content: space-between;
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
  background-color: grey;
}
</style>
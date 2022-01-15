<template>
  <div id="settings" class="flex">
    <div class="flex jc-center ai-center reload-icon">
      &#x21bb;
    </div>
    <select id="refresh-rate" ref="refreshInterval" @change="handleChangeRefreshInterval">
      <option value="10" :selected="refreshInterval === 10">10s</option>
      <option value="30" :selected="refreshInterval === 30">30s</option>
      <option value="60" :selected="refreshInterval === 60">1m</option>
      <option value="120" :selected="refreshInterval === 120">2m</option>
      <option value="300" :selected="refreshInterval === 300">5m</option>
      <option value="600" :selected="refreshInterval === 600">10m</option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'RefreshSettings',
  props: {
    defaultRefreshInterval: {
      type: Number,
      default: 60
    }
  },
  methods: {
    setRefreshInterval(seconds) {
      if (this.refreshHandler)
        clearInterval(this.refreshHandler);
      
      this.refreshInterval = seconds;
      let reference = this;
      this.refreshHandler = setInterval(function () {
        reference.refreshData();
      }, seconds * 1000);
    },
    refreshData() {
      this.$emit('refresh');
    },
    handleChangeRefreshInterval() {
      this.refreshData();
      this.setRefreshInterval(this.$refs.refreshInterval.value);
    }
  },
  created() {
    this.setRefreshInterval(this.defaultRefreshInterval);
  }
}
</script>

<style scoped>
#settings {
  position: fixed;
  left: 10px;
  bottom: 10px;
  background-color: #b9b9b9;
  border-radius: 0.2rem;
  border: 1px solid #b9b9b9;
  box-shadow: 0 1px 3px 0 rgba(0,0,0,.1);
}
.reload-icon {
  padding: 0;
  width: 1.4rem;
  height: 1.4rem;
  text-align: center;
}
#settings > select {
  font-size: 0.8rem;
  color: #424242;
  text-transform: none;
  border: 0px;
}
#settings select:focus {
  box-shadow: none;
}
</style>
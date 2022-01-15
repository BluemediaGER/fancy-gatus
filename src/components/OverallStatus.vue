<template>
  <div class="shadow-box">
    <div class="flex ai-center status-wrapper" v-if="this.failedEndpoints.length === 0">
      <BIconCheckCircleFill class="icon green" />
      <span>All services healthy.</span>
    </div>
    <div class="flex ai-center status-wrapper" v-else>
      <BIconExclamationCircleFill class="icon orange" />
      <span>{{ failedEndpointCountText }}</span>
    </div>
  </div>
</template>

<script>
import { BIconExclamationCircleFill, BIconCheckCircleFill } from 'bootstrap-icons-vue';

export default {
  name: 'OverallStatus',
  components: {
    BIconExclamationCircleFill,
    BIconCheckCircleFill
  },
  props: {
    failedEndpoints: {
      type: Array,
      required: true
    },
  },
  computed: {
    failedEndpointCountText() {
      if (this.failedEndpoints.length === 2) {
        return 'Two services are experiencing issues.';
      } else if (this.failedEndpoints.length === 3) {
        return 'Three services are experiencing issues.';
      } else if (this.failedEndpoints.length > 3) {
        return 'Multiple services are experiencing issues.';
      } else {
        return 'One service is experiencing issues.';
      }
    },
  }
}
</script>

<style scoped>
.status-wrapper {
  font-size: 1.3rem;
  margin-left: 10px;
  margin-top: 10px;
  margin-bottom: 10px;
}
.icon {
  font-size: 30px;
}
span {
  margin-left: 1rem;
}
</style>
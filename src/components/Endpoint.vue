<template>
  <div class="endpoint">
    <BIconCheckCircleFill class="green icon" v-if="isSuccessfull" />
    <BIconExclamationCircleFill class="orange icon" v-if="!isSuccessfull" />
    <span>{{ endpoint.name }}</span>
    <span class="hostname" v-if="hostname">| {{ hostname }}</span>
    <span class="spacer"></span>
    <EndpointHistory :results="endpoint.results" />
  </div>
</template>

<script>
import { BIconExclamationCircleFill, BIconCheckCircleFill } from 'bootstrap-icons-vue';
import EndpointHistory from '@/components/EndpointHistory.vue';

export default {
  name: 'Endpoint',
  components: {
    BIconExclamationCircleFill,
    BIconCheckCircleFill,
    EndpointHistory
  },
  props: {
    endpoint: {
      type: Object,
      required: true
    }
  },
  computed: {
    isSuccessfull() {
      return this.endpoint.results[this.endpoint.results.length - 1].success;
    },
    hostname() {
      return this.endpoint.results[0].hostname;
    }
  }
}
</script>

<style scoped>
.endpoint {
  display: flex;
  align-items: center;
  padding: 10px;
}
span {
  margin: 0;
  margin-left: 10px;
  margin-top: 2px;
}
.hostname {
  margin-left: 5px;
  color: rgb(143, 143, 143);
}
.icon {
  font-size: 20px;
}
.spacer {
  flex: 1;
}
</style>
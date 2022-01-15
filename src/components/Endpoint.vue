<template>
  <div class="flex ai-center endpoint">
    <div class="flex ai-center info">
      <BIconCheckCircleFill class="green icon" v-if="isSuccessfull" />
      <BIconExclamationCircleFill class="orange icon" v-if="!isSuccessfull" />
      <span>{{ endpoint.name }}</span>
      <span class="hostname" v-if="hostname">| {{ hostname }}</span>
    </div>
    <span class="spacer"></span>
    <div class="history">
      <EndpointHistory :results="endpoint.results" />
    </div>
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

@media screen and (max-width: 768px) {
  .endpoint {
    flex-direction: column;
    align-items: flex-start;
    padding: 5px;
  }
  .info {
    flex-direction: row;
    justify-content: flex-start;
  }
  .history {
    flex-direction: row;
    align-items: center;
    width: 100%;
  }
  .icon {
    font-size: 15px;
  }
  span {
    font-size: 15px;
  }
  .hostname {
    display: none;
  }
}
</style>
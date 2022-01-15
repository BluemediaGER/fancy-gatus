<template>
  <div>
    <h3>{{ name }}</h3>
    <div class="flex flex-column shadow-box">
      <Endpoint class="endpoint" v-for="endpoint in endpoints" :key="endpoint.name" :endpoint="endpoint" />
    </div>
  </div>
</template>

<script>
import Endpoint from '@/components/Endpoint.vue';

export default {
  name: 'EndpointGroup',
  components: {
    Endpoint
  },
  props: {
    name: {
      type: String,
      required: true
    },
    endpoints: {
      type: Array,
      required: true
    }
  },
}
</script>

<style scoped>
.endpoint {
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
}
</style>
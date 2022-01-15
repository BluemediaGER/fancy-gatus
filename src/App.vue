<template>
  <div class="main">
    <div v-if="$data.loading" class="loader-wrapper">
      <Loader />
    </div>
    <div v-if="!$data.loading" class="content-wrapper">
      <Header :title="this.config.title" />
      <OverallStatus class="overall-status" :failedEndpoints="failedEndpoints" />
      <EndpointGroup class="endpoint-group" v-for="(value, key) in groups" :key="key" :name="key" :endpoints="value" />
    </div>
    <Footer />
  </div>
</template>

<script>
import Loader from '@/components/Loader.vue';
import Header from '@/components/Header.vue';
import OverallStatus from '@/components/OverallStatus.vue';
import EndpointGroup from '@/components/EndpointGroup.vue';
import Footer from '@/components/Footer.vue';

import axios from 'axios';

export default {
  name: 'App',
  components: {
    Loader,
    Header,
    OverallStatus,
    EndpointGroup,
    Footer
  },
  data() {
    return {
      loading: true,
      config: null,
      apiData: null
    };
  },
  computed: {
    // Group endpoints by their group name
    groups() {
      return this.apiData.reduce(function(rv, x) {
        (rv[x["group"]] = rv[x["group"]] || []).push(x);
        return rv;
      }, {});
    },
    // Get an array of all failed endpoints
    failedEndpoints() {
      return this.apiData.filter(endpoint => {
        return !endpoint.results[endpoint.results.length - 1].success;
      });
    }
  },
  methods: {
    // Get frontend config and trigger initial data fetch
    getConfig() {
      axios.get('/config.json')
        .then(response => {
          this.config = response.data;
          // Set title if defined in config
          if (this.config.title) {
            document.title = this.config.title;
          }
          this.getApiData();
        })
        .catch(error => {
          console.log(error);
        });
    },
    getApiData() {
      // Set base URL for API calls if defined in config
      if (this.config.gatusBaseUrl && axios.defaults.baseURL !== this.config.gatusBaseUrl) {
        axios.defaults.baseURL = this.config.gatusBaseUrl;
      }
      axios.get('/api/v1/endpoints/statuses')
        .then(response => {
          // Remove hidden endpoints if defined in config
          if (this.config.hiddenEndpoints) {
            this.apiData = response.data.filter(endpoint => {
              return !this.config.hiddenEndpoints.includes(endpoint.name);
            });
          } else {
            this.apiData = response.data;
          }
          this.loading = false;
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  mounted() {
    this.getConfig();
  }
}
</script>

<style scoped>
.main {
  padding-left: 30px;
  padding-right: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.loader-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.content-wrapper {
  min-width: 60%;
  max-width: 90%;
  margin-top: 1rem;
  margin-bottom: 5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.overall-status {
  margin-top: 2rem;
}
.endpoint-group {
  margin-top: 2rem;
}
</style>

<style>

body,
html {
  padding: 0;
  margin: 0;
  --green: #00d560;
  --orange: #ff9100;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2F3545;
  padding: 0;
}
.shadow-box {
  box-shadow: 0 15px 70px rgb(0 0 0 / 10%);
  padding: 10px;
  border-radius: 10px;
}
.green {
  color: var(--green);
}
.orange {
  color: var(--orange);
}
</style>
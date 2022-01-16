<template>
  <div class="flex flex-column ai-center main">
    <div v-if="$data.loading" class="flex ai-center jc-center loader-wrapper">
      <Loader />
    </div>
    <div v-if="!$data.loading" class="flex flex-column jc-center content-wrapper">
      <Header :title="this.config.title" />
      <OverallStatus class="mtop-2" :failedEndpoints="failedEndpoints" />
      <EndpointGroup class="mtop-2" v-for="(value, key) in groups" :key="key" :name="key" :endpoints="value" />
    </div>
    <div v-if="!$data.loading">
      <RefreshSettings class="refresh-settings" :defaultRefreshInterval="$data.config.defaultRefreshInterval" v-on:refresh="this.getApiData()" />
      <Footer />
    </div>
  </div>
</template>

<script>
import Loader from '@/components/Loader.vue';
import Header from '@/components/Header.vue';
import OverallStatus from '@/components/OverallStatus.vue';
import EndpointGroup from '@/components/EndpointGroup.vue';
import RefreshSettings from '@/components/RefreshSettings.vue';
import Footer from '@/components/Footer.vue';

import axios from 'axios';

export default {
  name: 'App',
  components: {
    Loader,
    Header,
    OverallStatus,
    EndpointGroup,
    RefreshSettings,
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
    // and sort the groups according to the config if specified
    groups() {
      // Group
      let groups = this.apiData.reduce(function(rv, x) {
        if (!x.group) {
          x.group = "Ungrouped";
        }
        (rv[x["group"]] = rv[x["group"]] || []).push(x);
        return rv;
      }, {});
      // Sort
      if (this.config.groupOrder) {
        let order = [...this.config.groupOrder];
        let tmp = {};
        Object.assign(tmp, groups);
        groups = {};
        order.forEach(key => {
          if (key in tmp) {
            groups[key] = tmp[key];
            delete tmp[key];
          }
        });
        Object.assign(groups, tmp);
      }
      return groups;
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
      axios.get('config.json')
        .then(response => {
          this.config = response.data;
          // Set title if defined in config
          if (this.config.title) {
            document.title = this.config.title;
          }
          this.getApiData();
        })
        .catch(error => {
          if (error.response.status === 404) {
            console.warn('Could not find config.json. Using default values.');
            this.config = {}
            this.getApiData();
          } else {
            console.log("Error getting config: " + error);
          }
        });
    },
    getApiData() {
      // Set base URL for API calls if defined in config
      if (this.config.gatusBaseUrl && axios.defaults.baseURL !== this.config.gatusBaseUrl) {
        axios.defaults.baseURL = this.config.gatusBaseUrl;
      }
      axios.get('/api/v1/endpoints/statuses')
        .then(response => {
          let data = response.data;
          // Remove hidden groups if defined in config
          if (this.config.hiddenGroups) {
            data = data.filter(endpoint => {
              return !this.config.hiddenGroups.includes(endpoint.group);
            });
          }
          // Remove hidden endpoints if defined in config
          if (this.config.hiddenEndpoints) {
            data = data.filter(endpoint => {
              return !this.config.hiddenEndpoints.includes(endpoint.name);
            });
          }
          this.apiData = data;
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
.mtop-2 {
  margin-top: 2rem;
}
.main {
  padding-left: 30px;
  padding-right: 30px;
}
.loader-wrapper {
  height: 100vh;
}
.content-wrapper {
  width: 65%;
  max-width: 1320px;
  margin-top: 1rem;
  margin-bottom: 3rem;
}
@media screen and (max-width: 768px) {
  .main {
    padding-left: 15px;
    padding-right: 15px;
  }
  .content-wrapper {
    width: 100%;
  }
  .refresh-settings {
    display: none;
  }
}
</style>

<style>

body,
html {
  padding: 0;
  margin: 0;
  --green: #00d560;
  --orange: #ff9100;
  --grey: #7d8187;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2F3545;
  padding: 0;
}
.flex {
  display: flex;
}
.jc-center {
  justify-content: center;
}
.jc-space-between {
  justify-content: space-between;
}
.ai-center {
  align-items: center;
}
.flex-column {
  flex-direction: column;
}
.flex-row {
  flex-direction: row;
}
.shadow-box {
  box-shadow: 2px 3px 10px 2px rgb(0 0 0 / 10%);
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
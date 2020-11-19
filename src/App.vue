<template>
  <div id="app">
    <h1 class="title">Teste paginação</h1>
    <div class="list">
      <div v-for="passenger in passengers" :key="passenger._id" class="card">
        <strong>{{ passenger.name }}</strong>
        <span class="tag">{{ passenger.trips }}</span>
      </div>
    </div>
    <pagination
      v-if="passengers.length"
      :offset="offset"
      :total="total"
      :limit="limit"
      @change-page="changePage"
    />
  </div>
</template>

<script>
import Pagination from './components/Pagination.vue';

const axios = require('axios');

export default {
  name: 'App',
  components: {
    Pagination,
  },
  data() {
    return {
      passengers: [],
      offset: 0,
      total: 0,
      limit: 50,
    };
  },
  methods: {
    changePage(value) {
      this.offset = value;
      this.getPassengers();
    },
    getPassengers() {
      const BASE_URL = 'https://api.instantwebtools.net';
      const url = `${BASE_URL}/v1/passenger?page=${this.offset}&size=${this.limit}`;

      axios.get(url).then(({ data }) => {
        this.passengers = data.data;
        this.total = data.totalPassengers;
      });
    },
  },
  created() {
    this.getPassengers();
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.title {
  margin-bottom: 2rem;
}

.list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin-bottom: 1rem;

  .card {
    width: 15%;
    border: 1px solid #ccc;
    border-radius: 0.25rem;
    margin-bottom: 1rem;
    padding: 1rem;

    .tag {
      display: inline-block;
      background-color: #999;
      color: #fff;
      border-radius: 0.25rem;
      padding: 0.15rem 0.25rem;
      font-size: 0.75rem;
      margin-left: 0.5rem;
    }
  }
}
</style>

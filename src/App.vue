<template>
  <div class="test">
    <h1 class="title">Teste paginação</h1>
    <div class="list">
      <div v-for="item in results" :key="item._id" class="card">
        <strong>{{ item.name }}</strong>
        <span class="tag">{{ item.trips }}</span>
      </div>
    </div>
    <pagination
      v-if="results.length"
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
      results: [],
      offset: 0,
      total: 0,
      limit: 40,
    };
  },
  methods: {
    changePage(value) {
      this.offset = value;
      this.getPassengers();
    },
    getPassengers() {
      const url = `https://api.instantwebtools.net/v1/passenger?page=${this.offset}&size=${this.limit}`;

      axios
        .get(url)
        .then(({ data }) => {
          this.results = data.data;
          this.total = data.totalPassengers;
        })
        .catch(function (error) {
          // handle error
          console.log(error);
        });
    },
  },
  async created() {
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

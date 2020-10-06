<template>
  <div class="pagination">
    <span v-if="showPrevious" class="item prev" @click="changePage(current - 1)"
      >&laquo;</span
    >
    <span
      v-for="page in pages"
      :key="page"
      class="item"
      :class="{ current: page === current }"
      @click="changePage(page)"
      >{{ page }}</span
    >
    <span v-if="showNext" class="item next" @click="changePage(current + 1)"
      >&raquo;</span
    >
  </div>
</template>

<script>
export default {
  name: 'IxPagination',
  props: {
    offset: {
      type: [String, Number],
      default: 0,
    },
    total: {
      type: [String, Number],
      required: true,
    },
    limit: {
      type: [String, Number],
      default: 10,
    },
  },
  computed: {
    showPrevious() {
      return this.current > 1;
    },
    showNext() {
      return this.total > this.limit * this.current;
    },
    current() {
      return this.offset ? this.offset + 1 : 1;
    },
    pages() {
      const qty = Math.ceil(this.total / this.limit);

      if (qty <= 1) return [1];

      return Array.from(Array(qty).keys()).map((i) => i + 1);
    },
  },
  methods: {
    changePage(page) {
      this.$emit('change-page', page - 1);
    },
  },
};
</script>

<style lang="scss" scoped>
$light-grey: #cccccc;
$dark-grey: #333333;
$orange: #f90;
$dark-orange: #f63;

.pagination {
  display: flex;
  justify-content: center;

  .item {
    padding: 0.5rem 0.75rem;
    border: 1px solid $light-grey;
    cursor: pointer;
    background-color: white;

    &:first-child {
      border-top-left-radius: 3px;
      border-bottom-left-radius: 3px;
    }

    &:last-child {
      border-top-right-radius: 3px;
      border-bottom-right-radius: 3px;
    }

    &:hover {
      background-color: $light-grey;
      border-color: lighten($dark-grey, 50%);
      z-index: 3;
    }

    &.current {
      cursor: default;
      color: white;
      background-color: $orange;
      border-color: $dark-orange;
      z-index: 2;
    }

    + .item {
      margin-left: -1px;
      margin-right: 0;
    }
  }
}
</style>

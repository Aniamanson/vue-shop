<template>
  <ul class="catalog__pagination pagination" v-if="pages > 0">
    <li class="pagination__item">
      <a
        href="#"
        class="pagination__link pagination__link--arrow"
        aria-label="Предыдущая страница"
        :class="{ 'pagination__link--disabled': page === 1 }"
        @click.prevent="paginate(page > 1 ? page - 1 : page)"
      >
        <svg width="8" height="14" fill="currentColor">
          <use xlink:href="#icon-arrow-left"></use>
        </svg>
      </a>
    </li>

    <li
      class="pagination__item"
      v-for="pagintationTrigger in paginationTriggers"
      :key="pagintationTrigger"
    >
      <a
        href="#"
        class="pagination__link"
        :class="{ 'pagination__link--current': pagintationTrigger === page }"
        @click.prevent="paginate(pagintationTrigger)"
      >
        {{ pagintationTrigger }}
      </a>
    </li>

    <li class="pagination__item">
      <a
        class="pagination__link pagination__link--arrow"
        href="#"
        :class="{ 'pagination__link--disabled': page === pages }"
        @click.prevent="paginate(page < pages ? page + 1 : page)"
      >
        <svg width="8" height="14" fill="currentColor">
          <use xlink:href="#icon-arrow-right"></use>
        </svg>
      </a>
    </li>
  </ul>
</template>

<script>
export default {
  name: 'BasePagination',
  model: {
    prop: 'page',
    event: 'paginate',
  },
  props: ['page', 'perPage', 'count', 'visiblePagesCount'],
  data: () => ({
    currentPage: 1,
  }),
  computed: {
    pages() {
      return Math.ceil(this.count / this.perPage);
    },
    paginationTriggers() {
      const pageCount = this.pages;
      const visiblePagesThreshold = (this.visiblePagesCount - 1) / 2;
      const pagintationTriggersArray = Array(this.visiblePagesCount - 1).fill(0);
      let pagintationTriggers = [];

      this.firstPage();
      if (pageCount <= this.visiblePagesCount) {
        for (let i = 1; i < pageCount + 1; i += 1) {
          pagintationTriggers.push(i);
        }
        return pagintationTriggers;
      }

      if (this.currentPage <= visiblePagesThreshold + 1) {
        pagintationTriggersArray[0] = 1;
        pagintationTriggers = pagintationTriggersArray.map(
          (paginationTrigger, index) => pagintationTriggersArray[0] + index,
        );
        pagintationTriggers.push('...', pageCount);
        return pagintationTriggers;
      }
      if (this.currentPage >= pageCount - visiblePagesThreshold - 1) {
        pagintationTriggers = pagintationTriggersArray.map(
          (paginationTrigger, index) => pageCount - index,
        );
        pagintationTriggers.reverse().unshift(1, '...');
        return pagintationTriggers;
      }
      pagintationTriggersArray[0] = this.currentPage - visiblePagesThreshold + 1;
      pagintationTriggers = (pagintationTriggersArray).map(
        (paginationTrigger, index) => pagintationTriggersArray[0] + index,
      );
      pagintationTriggers.unshift(1, '...');
      pagintationTriggers.push('...', pageCount);
      return pagintationTriggers;
    },
  },
  methods: {
    paginate(clickPage) {
      this.$emit('paginate', clickPage);
      this.currentPage = clickPage;
    },
    firstPage() {
      if (this.page === 1) {
        this.currentPage = 1;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.pagination__link--current {
  cursor: default;
  &:hover,
  &:active,
  &:focus {
    color: #222 !important;
    opacity: 1 !important;
  }
}
</style>

/* eslint-disable max-len */
<template>
  <main class="content container">
    <div class="content__top content__top--catalog">
      <h1 class="content__title">Каталог</h1>
      <span class="content__info">
        {{ casesChange }}
      </span>
    </div>

    <div class="content__catalog">
      <ProductFilter
        :price-from.sync="filterPriceFrom"
        :price-to.sync="filterPriceTo"
        :category-id.sync="filterCategoryId"
        :price-sort.sync="priceSort"
      />
      <section class="catalog">
        <div class="catalog-alert" v-if="filterProducts.length === 0">
          По вашему запросу ничего не найдено
          <div class="emoji">&#129302;</div>
          Пожалуйста, измените параметры фильтрации!
        </div>
        <ProductList :products="products" v-else />
        <BasePagination
          v-model="page"
          :per-page="productPerPage"
          :count="countProducts"
          :visible-pages-count="visiblePagesCount"
        />
      </section>
    </div>
  </main>
</template>

<script>
import products from './data/products';
import cases from './data/cases';
import ProductList from './components/ProductList.vue';
import BasePagination from './components/Pagination.vue';
import ProductFilter from './components/ProductFilter.vue';

export default {
  name: 'App',
  components: {
    ProductList,
    BasePagination,
    ProductFilter,
  },
  data: () => ({
    page: 1,
    productPerPage: 3,
    visiblePagesCount: 5,
    filterPriceFrom: '',
    filterPriceTo: '',
    filterCategoryId: 0,
    priceSort: '',
  }),
  computed: {
    filterProducts() {
      let filterProducts = products;
      const priceFrom = Number(this.filterPriceFrom);
      const priceTo = Number(this.filterPriceTo);

      if (priceFrom > 0 && priceTo > 0) {
        if (priceFrom <= priceTo) {
          filterProducts = filterProducts.filter((product) => (product.price >= priceFrom)
          && (product.price <= priceTo));
          this.sort(filterProducts);
        }
      }

      if (priceFrom > 0 && priceTo === 0) {
        filterProducts = filterProducts.filter((product) => product.price >= priceFrom);
      }
      if (priceFrom === 0 && priceTo > 0) {
        filterProducts = filterProducts.filter((product) => product.price <= priceTo);
      }

      if (this.filterCategoryId) {
        filterProducts = filterProducts.filter(
          (product) => product.categoryId === this.filterCategoryId,
        );
      }

      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      this.page = 1;

      return this.sort(filterProducts);
    },
    products() {
      const offset = (this.page - 1) * this.productPerPage;
      return this.filterProducts.slice(offset, offset + this.productPerPage);
    },
    countProducts() {
      return this.filterProducts.length;
    },
    casesChange() {
      const caseWord = `${this.countProducts} ${
        cases(this.countProducts, ['товар', 'товара', 'товаров'])}`;
      return caseWord;
    },
  },
  methods: {
    sort(arr) {
      let sortProducts = arr;
      function byField(field) {
        return (a, b) => (a[field] > b[field] ? 1 : -1);
      }
      function byFieldDown(field) {
        return (a, b) => (b[field] > a[field] ? 1 : -1);
      }
      if (this.priceSort === 'up') {
        sortProducts = sortProducts.sort(byField('price'));
      } else if (this.priceSort === 'down') {
        sortProducts = sortProducts.sort(byFieldDown('price'));
      } else {
        return sortProducts;
      }
      return sortProducts;
    },
  },
};
</script>

<style lang="scss">
.catalog__pic img {
  width: 100%;
  height: 100%;
  -o-object-fit: contain !important;
  object-fit: contain !important;
}
.catalog-alert {
  max-width: 800px;
  margin: 0 auto;
  font-size: 20px;
  font-weight: bold;
  // font-family: "PressStart";
  color: rgba(20, 94, 151, 0.9);
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  justify-content: center;
  .emoji {
    margin-bottom: 6px;
    margin-left: 10px;
    font-size: 30px;
  }
}
</style>

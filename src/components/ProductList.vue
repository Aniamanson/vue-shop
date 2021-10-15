<template>
  <ul class="catalog__list">
    <ProductItem
      :product="product"
      v-for="product in products"
      :key="product.title"
    />
  </ul>
</template>

<script>
import ProductItem from './ProductItem.vue';

export default {
  name: 'ProductList',
  components: {
    ProductItem,
  },
  props: ['products'],
  data: () => ({
  }),
};
</script>

<style scoped>
</style>

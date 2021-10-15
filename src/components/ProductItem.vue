<template>
  <li>
    <a class="catalog__pic" href="#">
      <img :src="product.img" :alt="product.title" />
    </a>

    <h3 class="catalog__title">
      <a href="#"> {{ product.title }} </a>
    </h3>

    <span class="catalog__price"> {{ priceComputed }} ₽ </span>

    <!-- <ul class="colors colors--black">
      <li class="colors__item">
        <label class="colors__label">
          <input
            class="colors__radio sr-only"
            type="radio"
            value="#73B6EA"
            v-model="color"
          />
          <span class="colors__value" style="background-color: #73b6ea"> </span>
        </label>
      </li>
      <li class="colors__item">
        <label class="colors__label">
          <input
            class="colors__radio sr-only"
            type="radio"
            value="#8BE000"
            v-model="color"
          />
          <span class="colors__value" style="background-color: #8be000"> </span>
        </label>
      </li>
      <li class="colors__item">
        <label class="colors__label">
          <input
            class="colors__radio sr-only"
            type="radio"
            value="#222"
            v-model="color"
          />
          <span class="colors__value" style="background-color: #222"> </span>
        </label>
      </li>
    </ul> -->
  </li>
</template>

<script>
export default {
  name: 'ProductList',
  props: ['product'],
  data: () => ({
    color: '#73B6EA',
  }),
  computed: {
    priceComputed() {
      const price = this.product.price.toLocaleString('ru-RU');
      return price;
    },
  },
  methods: {
  },
};
</script>

<style scoped>
</style>

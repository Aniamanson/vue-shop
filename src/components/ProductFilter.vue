<template>
  <aside class="filter">
    <h2 class="filter__title">Фильтры</h2>

    <form
      class="filter__form form"
      action="#"
      method="get"
      @submit.prevent="submit"
    >
      <fieldset class="form__block">
        <legend class="form__legend">Сортировать:</legend>
        <ul class="price-filter">
          <li class="price-filter__item">
            <label class="price-filter__label">
              <input
                class="price-filter__radio sr-only"
                type="radio"
                name="price-filter"
                value="up"
                v-model="priceSort"
                @change="sort"
              />
              <span class="price-filter__value"> </span>
              Возрастанию цены
            </label>
          </li>
          <li class="price-filter__item">
            <label class="price-filter__label">
              <input
                class="price-filter__radio sr-only"
                type="radio"
                name="price-filter"
                value="down"
                v-model="priceSort"
                @change="sort"
              />
              <span class="price-filter__value"> </span>
              Убыванию цены
            </label>
          </li>
        </ul>
      </fieldset>
      <fieldset class="form__block">
        <legend class="form__legend">Цена</legend>
        <label class="form__label form__label--price">
          <input
            class="form__input"
            type="text"
            name="min-price"
            placeholder="0"
            v-model="currentPriceFrom"
            @keyup="validationPrice"
          />
          <span class="form__value">От</span>
          <span class="error"></span>
        </label>
        <label class="form__label form__label--price">
          <input
            class="form__input"
            type="text"
            name="max-price"
            placeholder="0"
            v-model="currentPriceTo"
            @keypress="validationPrice"
          />
          <span class="form__value">До</span>
          <span class="form__label-error" v-if="errorInput === true">
            Цена "до" должна быть больше цены "от"
          </span>
        </label>
      </fieldset>

      <fieldset class="form__block">
        <legend class="form__legend">Категория</legend>
        <label class="form__label form__label--select">
          <select
            class="form__select"
            type="text"
            name="category"
            v-model.number="currentCategoryId"
            @change="sort"
          >
            <option value="0">Все категории</option>
            <option
              :value="category.id"
              v-for="category in categories"
              :key="category.id"
              @select="FilterByCategory(category.id)"
            >
              {{ category.title }}
            </option>
          </select>
        </label>
      </fieldset>

      <!-- <fieldset class="form__block">
        <legend class="form__legend">Цвет</legend>
        <ul class="colors">
          <li class="colors__item">
            <label class="colors__label">
              <input
                class="colors__radio sr-only"
                type="radio"
                name="color"
                value="#73B6EA"
                checked=""
              />
              <span class="colors__value" style="background-color: #73b6ea">
              </span>
            </label>
          </li>
          <li class="colors__item">
            <label class="colors__label">
              <input
                class="colors__radio sr-only"
                type="radio"
                name="color"
                value="#FFBE15"
              />
              <span class="colors__value" style="background-color: #ffbe15">
              </span>
            </label>
          </li>
          <li class="colors__item">
            <label class="colors__label">
              <input
                class="colors__radio sr-only"
                type="radio"
                name="color"
                value="#939393" />
              <span class="colors__value" style="background-color: #939393">
              </span
            ></label>
          </li>
          <li class="colors__item">
            <label class="colors__label">
              <input
                class="colors__radio sr-only"
                type="radio"
                name="color"
                value="#8BE000" />
              <span class="colors__value" style="background-color: #8be000">
              </span
            ></label>
          </li>
          <li class="colors__item">
            <label class="colors__label">
              <input
                class="colors__radio sr-only"
                type="radio"
                name="color"
                value="#FF6B00" />
              <span class="colors__value" style="background-color: #ff6b00">
              </span
            ></label>
          </li>
          <li class="colors__item">
            <label class="colors__label">
              <input
                class="colors__radio sr-only"
                type="radio"
                name="color"
                value="#FFF" />
              <span class="colors__value" style="background-color: #fff"> </span
            ></label>
          </li>
          <li class="colors__item">
            <label class="colors__label">
              <input
                class="colors__radio sr-only"
                type="radio"
                name="color"
                value="#000" />
              <span class="colors__value" style="background-color: #000"> </span
            ></label>
          </li>
        </ul>
      </fieldset>

      <fieldset class="form__block">
        <legend class="form__legend">Объемб в ГБ</legend>
        <ul class="check-list">
          <li class="check-list__item">
            <label class="check-list__label">
              <input
                class="check-list__check sr-only"
                type="checkbox"
                name="volume"
                value="8"
                checked=""
              />
              <span class="check-list__desc">
                8
                <span>(313)</span>
              </span>
            </label>
          </li>
          <li class="check-list__item">
            <label class="check-list__label">
              <input
                class="check-list__check sr-only"
                type="checkbox"
                name="volume"
                value="16"
              />
              <span class="check-list__desc">
                16
                <span>(461)</span>
              </span>
            </label>
          </li>
          <li class="check-list__item">
            <label class="check-list__label">
              <input
                class="check-list__check sr-only"
                type="checkbox"
                name="volume"
                value="32"
              />
              <span class="check-list__desc">
                32
                <span>(313)</span>
              </span>
            </label>
          </li>
          <li class="check-list__item">
            <label class="check-list__label">
              <input
                class="check-list__check sr-only"
                type="checkbox"
                name="volume"
                value="64"
              />
              <span class="check-list__desc">
                64
                <span>(313)</span>
              </span>
            </label>
          </li>
          <li class="check-list__item">
            <label class="check-list__label">
              <input
                class="check-list__check sr-only"
                type="checkbox"
                name="volume"
                value="128"
              />
              <span class="check-list__desc">
                128
                <span>(313)</span>
              </span>
            </label>
          </li>
          <li class="check-list__item">
            <label class="check-list__label">
              <input
                class="check-list__check sr-only"
                type="checkbox"
                name="volume"
                value="264"
              />
              <span class="check-list__desc">
                264
                <span>(313)</span>
              </span>
            </label>
          </li>
        </ul>
      </fieldset> -->

      <button class="filter__submit button button--primery" type="submit">
        Применить
      </button>
      <button
        class="filter__reset button button--second"
        type="button"
        @click.prevent="reset"
      >
        Сбросить
      </button>
    </form>
  </aside>
</template>

<script>
import categories from '../data/categories';

export default {
  name: 'ProductFilter',
  components: {
  },
  props: ['priceFrom', 'priceTo', 'categoryId', 'priceSort'],
  data: () => ({
    currentPriceTo: '',
    currentPriceFrom: '',
    currentCategoryId: 0,
    errorInput: false,
    priceSort: '',
  }),
  computed: {
    categories() {
      return categories;
    },
  },
  methods: {
    validationPrice(e) {
      if (e.target.getAttribute('name') === 'min-price') {
        console.log(e.target.value);
        if (e.target.value === 'e' || e.target.value === '-') {
          e.target.value = '';
          this.currentPriceFrom = e.target;
        }
        this.currentPriceFrom = e.target.value.replace(/[^0-9]/g, '');
      }
      if (e.target.getAttribute('name') === 'max-price') {
        if (e.target.value === 'e' || e.target.value === '-') {
          e.target.value = '';
          this.currentPriceTo = e.target.value;
        }
        this.currentPriceTo = e.target.value.replace(/[^0-9]/g, '');
      }
      this.currentPriceFrom = Number(this.currentPriceFrom);
      this.currentPriceTo = Number(this.currentPriceTo);
    },
    sort() {
      this.$emit('update:priceSort', this.priceSort);
      this.$emit('update:categoryId', this.currentCategoryId);
    },
    submit() {
      this.$emit('update:priceFrom', this.currentPriceFrom);
      this.$emit('update:priceTo', this.currentPriceTo);
      this.$emit('update:categoryId', this.currentCategoryId);
      this.$emit('update:priceSort', this.priceSort);
      if ((this.currentPriceFrom > 0 && this.currentPriceTo > 0)
        && (this.currentPriceTo < this.currentPriceFrom)) {
        this.errorInput = true;
      } else {
        this.errorInput = false;
      }
    },
    reset() {
      this.$emit('update:priceFrom', '');
      this.$emit('update:priceTo', '');
      this.$emit('update:categoryId', '');
      this.$emit('update:priceSort', '');
      this.currentPriceTo = '';
      this.currentPriceFrom = '';
      this.currentCategoryId = 0;
      this.priceSort = '';
    },
  },
};
</script>

<style lang="scss" scoped>
.form__label-error {
  color: red;
  position: absolute;
  left: 0;
  bottom: -20px;
  font-size: 12px;
}
.form__input.error {
  border: 1px solid red;
}
.price-filter {
  margin: 0;
  padding: 0;
  list-style: none;
  --border-color: #fff;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  -ms-flex-wrap: wrap;
  flex-wrap: wrap;

  &__item {
    margin-bottom: 10px;
  }

  &__label {
    position: relative;
    cursor: pointer;
    display: block;
    border-radius: 50%;
    padding: 3px;
    display: flex;
    align-items: center;
  }
  &__value {
    display: block;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    -webkit-transition: all 0.2s ease;
    transition: all 0.2s ease;
    background: white;
    margin-right: 10px;
    position: relative;

    &::before {
      content: "";
      position: absolute;
      top: 50%;
      left: 50%;
      -webkit-transform: translate(-50%, -50%) scale(0);
      transform: translate(-50%, -50%) scale(0);
      width: 15px;
      height: 15px;
      background: #0d910d;
      border-radius: 50%;
      transition: all 0.2s ease;
    }
  }

  input:checked + .price-filter__value::before {
    transform: translate(-50%, -50%) scale(1);
  }
}
</style>

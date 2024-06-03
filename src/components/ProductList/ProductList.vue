<template>
  <div>
    <div class="products-wrapper" v-if="filteredProducts.length > 0">
      <div
        v-for="product in filteredProducts"
        :key="product.id"
        class="product"
      >
        <img
          :src="getImagePath(product.image)"
          :alt="product.title"
          class="product-image"
        />
        <h3>{{ product.title }}</h3>
        <p>Brand {{ product.brand }}</p>
        <p>
          {{ product.regular_price.currency }} {{ product.regular_price.value }}
        </p>
        <button class="add-cart" @click="addToCart(product)">
          Add to Cart
        </button>
      </div>
    </div>
    <div class="no-brands" v-else>No brands</div>
  </div>
</template>

<script>
export default {
  props: {
    products: Array,
  },
  data() {
    return {
      selectedBrand: null,
    };
  },
  computed: {
    filteredProducts() {
      if (!this.selectedBrand) {
        return this.products;
      }
      return this.products.filter(
        (product) => product.brand === this.selectedBrand
      );
    },
  },
  methods: {
    addToCart(product) {
      this.$emit("addToCart", product);
    },
    getImagePath(image) {
      return `../src/assets${image}`;
    },
    filterProducts(brand) {
      this.selectedBrand = brand;
    },
  },
  watch: {
    selectedBrand(newBrand) {
      this.$emit("filterProducts", newBrand);
    },
  },
};
</script>

<style lang="scss">
@import "./ProductList.scss";
</style>

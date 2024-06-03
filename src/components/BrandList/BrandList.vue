<template>
  <div>
    <ul class="brands-wrapper" v-if="brands.length > 0">
      <li @click="showAllBrands" class="all-brands">All brands</li>
      <li
        v-for="brand in brands"
        :key="brand.id"
        @click="filterProducts(brand.id)"
      >
        {{ brand.title }}
      </li>
    </ul>
    <div class="no-brands" v-else>No brands</div>
  </div>
</template>

<script>
export default {
  props: {
    brands: Array,
  },
  methods: {
    filterProducts(brandId) {
      this.$emit("filterProducts", brandId);
    },
    showAllBrands() {
      this.$emit("filterProducts", null);
    },
  },
};
</script>
<style lang="scss">
@import "./BrandList.scss";
p {
  text-align: center;
  color: #666;
}
</style>

<template>
  <div id="app">
    <header>
      <img src="/assets/logo.png" alt="logo" class="logo" />
      <div class="cart-nav" @click="toggleCartModal">
        <span>🛒</span> <span>{{ cartTotalItems }}</span>
      </div>
    </header>
    <main>
      <aside>
        <BrandList :brands="brands" @filterProducts="filterProducts" />
      </aside>
      <section>
        <ProductList
          class="products"
          :products="filteredProducts"
          @addToCart="addToCart"
          :key="filteredProducts.length"
        />
      </section>
    </main>
    <div v-if="isCartModalVisible" class="modal">
      <div class="modal-content">
        <span class="close" @click="toggleCartModal">&times;</span>
        <Cart
          :cart="cart"
          @updateQuantity="updateQuantity"
          @removeItem="removeItem"
        />
      </div>
    </div>
  </div>
</template>

<script>
import BrandList from "./components/BrandList/BrandList.vue";
import ProductList from "./components/ProductList/ProductList.vue";
import Cart from "./components/Cart/Cart.vue";

import products from "./assets/products.json";
import brands from "./assets/brands.json";

export default {
  components: {
    BrandList,
    ProductList,
    Cart,
  },
  data() {
    return {
      products: [],
      brands: [],
      filteredProducts: [],
      cart: [],
      isCartModalVisible: false,
    };
  },
  computed: {
    cartTotalItems() {
      return this.cart.reduce((acc, item) => acc + item.quantity, 0);
    },
  },
  mounted() {
    this.products = products;
    this.brands = brands;
    this.filteredProducts = products;
  },
  methods: {
    filterProducts(brandId) {
      if (brandId === null) {
        this.filteredProducts = this.products;
      } else {
        this.filteredProducts = this.products.filter((product) => {
          return product.brand === brandId;
        });
      }
    },
    addToCart(product) {
      const existingProduct = this.cart.find((item) => item.id === product.id);
      if (existingProduct) {
        existingProduct.quantity++;
      } else {
        this.cart.push({ ...product, quantity: 1 });
      }
    },
    updateQuantity(updatedItem) {
      const item = this.cart.find((item) => item.id === updatedItem.id);
      if (item) {
        item.quantity = updatedItem.quantity;
      }
    },
    removeItem(itemId) {
      this.cart = this.cart.filter((item) => item.id !== itemId);
    },
    toggleCartModal() {
      this.isCartModalVisible = !this.isCartModalVisible;
    },
  },
};
</script>

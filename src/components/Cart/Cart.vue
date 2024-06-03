<template>
  <div>
    <div class="cart-header">
      <img src="/assets/logo.png" alt="logo" class="logo" />
    </div>
    <h2>Cart</h2>

    <div v-for="item in cart" :key="item.id" class="cart-item">
      <img
        :src="getImagePath(item.image)"
        :alt="item.title"
        class="cart-item-image"
      />
      <h3>{{ item.title }}</h3>
      <div class="quantity-controls">
        <button @click="decreaseQuantity(item)">-</button>
        <p>{{ item.quantity }}</p>
        <button @click="increaseQuantity(item)">+</button>
      </div>
      <p>
        {{ item.quantity }} x {{ item.regular_price.value }}
        {{ item.regular_price.currency }}
      </p>
      <span class="remove-cart" @click="removeItem(item.id)">🗑️</span>
    </div>
    <p>Total products: {{ totalItems }}</p>
    <p>
      Subtotal: {{ totalPrice }}
      {{ cart.length > 0 ? cart[0].regular_price.currency : "" }}
    </p>
    <button class="buy-cart" @click="checkout">Checkout</button>
  </div>
</template>

<script>
export default {
  props: {
    cart: Array,
  },
  computed: {
    totalItems() {
      return this.cart.reduce((acc, item) => acc + item.quantity, 0);
    },
    totalPrice() {
      return this.cart
        .reduce(
          (acc, item) => acc + item.quantity * item.regular_price.value,
          0
        )
        .toFixed(2);
    },
  },
  methods: {
    getImagePath(image) {
      return `../src/assets${image}`;
    },
    increaseQuantity(item) {
      item.quantity++;
      this.$emit("updateQuantity", item);
    },
    decreaseQuantity(item) {
      if (item.quantity > 1) {
        item.quantity--;
        this.$emit("updateQuantity", item);
      }
    },
    removeItem(itemId) {
      this.$emit("removeItem", itemId);
    },
    checkout() {
      console.log("The purchase has been completed");
    },
  },
};
</script>

<style lang="scss">
@import "./Cart.scss";
</style>

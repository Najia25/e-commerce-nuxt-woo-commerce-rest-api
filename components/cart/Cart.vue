<template>
  <v-card
  class="cart"
  min-width="322px"
  max-width="322px"
  >

    <div class="d-flex cart-header pa-2 align-center">
      <div class="d-flex align-center">
        <v-icon class="mr-2">mdi-shopping-outline</v-icon>
        <p class="mb-0" v-if="cart">{{ cart.totalProductsCount }} Items</p>
        <p class="mb-0" v-else>0 items</p>
      </div>
      <v-btn icon @click="cartClosed">
        <v-icon>mdi-close</v-icon>
      </v-btn>
    </div>

    <v-list v-if="cart" three-line class="cart-body">
      <CartItem
        v-for="(item, index) in cart.products"
        :key="item.productId"
        :cartItem="item"
        :index="index"
        :length="cart.products.length"
      />
    </v-list>
    <div v-else class="cart-body">
      <p>No items available in the cart</p>
      <button to="/" color="primary">Start shopping</button>
    </div>

    <div class="d-flex cart-footer pa-2 align-center justify-center">
      <div class="d-flex align-center">
        <v-btn v-if="cart" color="primary">Place Order | {{cart.totalProductPrice}} taka</v-btn>
      </div>
    </div>
  </v-card>
</template>

<script>
import CartItem from '@/components/cart/CartItem'
import { mapState } from 'vuex'
export default {
  components: {
    CartItem
  },
  data () {
    return {
      cartIsClosed: false
    }
  },
  computed: {
    ...mapState({
      cart: state => state.cart.cart
    })
  },
  methods: {
    cartClosed () {
      this.$emit('cart-closed')
      this.cartIsClosed = true
    }
  }
}
</script>

<style lang="scss" scoped>
.cart {
  position: fixed;
  right: 0%;
  z-index: 4;
}

.cart-header {
  background: #eee;
  position: fixed;
  height: 5vh;
  z-index: 5;
  justify-content: space-between;
  min-width: 322px;

  p{
    font-weight: 500;
    text-transform: uppercase;
    color: rgba(71, 67, 67, 0.932);
  }
}

.cart-body {
  padding-top: 5vh;
  overflow: auto;
  height: 87vh;
  padding-bottom: 8vh;
}

.cart-footer {
  background: #eee;
  position: fixed;
  height: 8vh;
  bottom: 0;
  min-width: 322px;
}

.animate__animated.animate__slideInRight {
  --animate-duration: 250ms;
  --animate-delay: 100ms;
}

</style>

<template>
  <div class="cart-item">
    <v-list-item class="px-0">
      <v-list-item-action class="mr-2 my-2">
        <v-btn small icon @click="handleQtyChange(added = true)">
          <v-icon small>
            mdi-chevron-up
          </v-icon>
        </v-btn>
        <v-list-item-action-text v-text="qty" class="item-quantity"></v-list-item-action-text>
        <v-btn small icon @click="handleQtyChange(added = false)" :disabled="qty <= 1">
          <v-icon small>
            mdi-chevron-down
          </v-icon>
        </v-btn>
      </v-list-item-action>

      <v-list-item-avatar class="mr-3 align-self-center" tile>
        <v-img :src="cartItem.image.sourceUrl"></v-img>
      </v-list-item-avatar>

      <v-list-item-content class="align-self-center">
        <v-list-item-subtitle v-text="cartItem.name" class="black--text"></v-list-item-subtitle>
        <v-list-item-action-text v-text="cartItem.price.toFixed(2)"></v-list-item-action-text>
      </v-list-item-content>

      <v-list-item-action class="align-self-center">
        <v-list-item-subtitle v-text="cartItem.totalPrice.toFixed(2)"></v-list-item-subtitle>
      </v-list-item-action>

      <v-list-item-action>
        <v-btn icon>
          <v-icon
            small
            color="grey lighten-1"
            @click="handleRemoveItem"
          >
            mdi-close
          </v-icon>
        </v-btn>
      </v-list-item-action>

    </v-list-item>
    <v-divider
      v-if="index < length - 1"
    ></v-divider>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import { updateCart, removeItemFromCart } from '@/functions'
import cloneDeep from 'lodash.clonedeep'
export default {
  props: ['cartItem', 'index', 'length'],
  data () {
    return {
      qty: this.cartItem.qty
    }
  },
  watch: {
    cart () {
      this.qty = this.cartItem.qty
    }
  },
  computed: {
    ...mapState({
      cart: state => state.cart.cart
    })
  },
  methods: {
    handleQtyChange (isAdded) {
      if (isAdded) {
        this.qty = this.qty + 1
      } else {
        this.qty = this.qty - 1
      }
      if (this.cart !== null) {
        // let existingCart = localStorage.getItem('cart')
        // existingCart = JSON.parse(existingCart)

        // due to object reference, passing this.cart gets cart state mutated in function.js outside of mutation
        // hence use new object and pass it

        const exisitingCartObj = cloneDeep(this.cart)
        const updatedCart = updateCart(exisitingCartObj, this.cartItem, false, this.qty)
        this.$store.commit('cart/add', updatedCart)
      }
    },
    handleRemoveItem () {
      const updatedCart = removeItemFromCart(this.cartItem.productId)
      this.$store.commit('cart/add', updatedCart)
    }
  }
}
</script>

<style scoped>
.v-list-item__action--stack {
  align-items: center;
}
.v-list-item .v-list-item__subtitle {
  /* -webkit-line-clamp: none !important; */
  display: block;
}
</style>

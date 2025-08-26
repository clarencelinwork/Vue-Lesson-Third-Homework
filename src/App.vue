<script setup>
import 'bootstrap/dist/css/bootstrap.min.css'
import 'popper.js'
import 'bootstrap'

import DrinkMenu from './components/DrinkMenu.vue'
import Cart from './components/Cart.vue'

import { ref, computed } from 'vue'

const cartItems = ref([])

const selectCount = ref([])

const cartTotal = computed(() => {
  return cartItems.value.reduce((total, item) => {
    return total + item.price * selectCount.value[item.id]
  }, 0)
})

const remark = ref()

const order = ref([])
const orderRemark = ref()
const orderTotal = ref(0)

const addToCart = (drink) => {
  let nowItem = cartItems.value.find((item) => item.id === drink.id)

  if (nowItem) {
    if (nowItem.count >= 10) {
      alert('一種飲料最多只能10杯')
    } else {
      nowItem.total = nowItem.total + drink.price
      nowItem.count += 1
      selectCount.value[drink.id] = nowItem.count
    }
  } else {
    drink.total = drink.price
    drink.count = 1
    selectCount.value[drink.id] = drink.count
    cartItems.value.push(drink)
  }
}

const removeCart = (cartItemId) => {
  cartItems.value = cartItems.value.filter((item) => item.id !== cartItemId)
}

const updateCartItemsTotal = (cartItem) => {
  cartItem.count = selectCount.value[cartItem.id]
}

function submitCartToOrder() {
  order.value = cartItems.value
  orderRemark.value = remark.value
  orderTotal.value = cartTotal.value

  cartItems.value = []
  selectCount.value = []
  remark.value = ''
}
</script>

<template>
  <div class="container p-5">
    <div class="row">
      <div class="col-4">
        <DrinkMenu @emit-drink="addToCart" />
      </div>
      <div class="col-8">
        <Cart
          :cart-items="cartItems"
          :select-count="selectCount"
          :cart-total="cartTotal"
          @emit-item-count="updateCartItemsTotal"
          @emit-remove-item="removeCart"
        />
        <textarea class="form-control" rows="5" placeholder="備註" v-model="remark"></textarea>
        <div class="text-right mt-2">
          <button class="btn btn-primary" @click="submitCartToOrder">送出</button>
        </div>
      </div>
      <div class="col-12">
        <hr />
      </div>
    </div>
    <div class="row justify-content-center">
      <div class="col-8 text-center" v-if="order.length > 0">
        <div>訂單</div>
        <table class="table table-bordered">
          <thead>
            <tr>
              <td>品項</td>
              <td>數量</td>
              <td>小記</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="orderItem in order" :key="orderItem.id">
              <td>{{ orderItem.name }}</td>
              <td>{{ orderItem.count }}</td>
              <td>{{ orderItem.total }}</td>
            </tr>
          </tbody>
          <tfoot class="text-right">
            <tr v-if="orderRemark">
              <td colspan="3">備註:{{ orderRemark }}</td>
            </tr>
            <tr>
              <td colspan="3">總計:{{ orderTotal }}</td>
            </tr>
          </tfoot>
        </table>
      </div>
    </div>
  </div>
</template>

<style scoped>
.total-font {
  font-size: 22px;
}
</style>

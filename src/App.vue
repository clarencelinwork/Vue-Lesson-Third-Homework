<script setup>
import 'bootstrap/dist/css/bootstrap.min.css'
import 'popper.js'
import 'bootstrap'

import { ref, computed } from 'vue'

const drinks = ref([
  {
    id: 1,
    name: '珍珠奶茶',
    description: '香濃奶茶搭配QQ珍珠',
    price: 50,
    count: 0,
    total: 0,
  },
  {
    id: 2,
    name: '冬瓜檸檬',
    description: '清新冬瓜配上新鮮檸檬',
    price: 45,
    count: 0,
    total: 0,
  },
  {
    id: 3,
    name: '翡翠檸檬',
    description: '綠茶與檸檬的完美結合',
    price: 55,
    count: 0,
    total: 0,
  },
  {
    id: 4,
    name: '四季春茶',
    description: '香醇四季春茶，回甘無比',
    price: 45,
    count: 0,
    total: 0,
  },
  {
    id: 5,
    name: '阿薩姆奶茶',
    description: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50,
    count: 0,
    total: 0,
  },
  {
    id: 6,
    name: '檸檬冰茶',
    description: '檸檬與冰茶的清新組合',
    price: 45,
    count: 0,
    total: 0,
  },
  {
    id: 7,
    name: '芒果綠茶',
    description: '芒果與綠茶的獨特風味',
    price: 55,
    count: 0,
    total: 0,
  },
  {
    id: 8,
    name: '抹茶拿鐵',
    description: '抹茶與鮮奶的絕配',
    price: 60,
    count: 0,
    total: 0,
  },
])

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

function addToCart(drink) {
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

function removeCart(drink) {
  cartItems.value = cartItems.value.filter((item) => item.id !== drink.id)
}

function updateCartItemsTotal(cartItem) {
  cartItem.count = selectCount.value[cartItem.id]
}

function getItemTotal(cartItem) {
  return cartItem.price * cartItem.count
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
        <div
          class="drink-list mouse-pointer"
          v-for="drink in drinks"
          :key="drink.id"
          @click="addToCart(drink)"
        >
          <div class="name-font">{{ drink.name }} ${{ drink.price }}</div>
          <div>{{ drink.description }}</div>
        </div>
      </div>
      <div class="col-8">
        <table class="table table-bordered">
          <thead>
            <tr class="text-center">
              <th>操作</th>
              <th>品項</th>
              <th>描述</th>
              <th>數量</th>
              <th>單價</th>
              <th>小計</th>
            </tr>
          </thead>
          <tbody class="text-center" v-if="cartItems.length > 0">
            <tr v-for="cartItem in cartItems" :key="cartItem.id">
              <td>
                <button type="button" @click="removeCart(cartItem)">X</button>
              </td>
              <td>{{ cartItem.name }}</td>
              <td>{{ cartItem.description }}</td>
              <td>
                <select
                  class="form-control"
                  @change="updateCartItemsTotal(cartItem)"
                  v-model="selectCount[cartItem.id]"
                >
                  <option v-for="itemCount in 10" :value="itemCount" :key="itemCount">
                    {{ itemCount }}
                  </option>
                </select>
              </td>
              <td>{{ cartItem.price }}</td>
              <td>{{ getItemTotal(cartItem) }}</td>
            </tr>
          </tbody>
          <tbody class="text-center" v-else>
            <tr>
              <td colspan="6">請選擇商品</td>
            </tr>
          </tbody>
          <tfoot class="text-right" v-if="cartItems.length > 0">
            <tr>
              <td class="total-font" colspan="6">總計:{{ cartTotal }}</td>
            </tr>
          </tfoot>
        </table>
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
      <div class="col-8 text-center " v-if="order.length > 0">
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
.drink-list {
  border: 1px solid #000;
  border-radius: 5px;
  padding: 10px;
}

.name-font {
  font-size: 20px;
}

.total-font {
  font-size: 22px;
}

.mouse-pointer {
  cursor: pointer;
}
</style>

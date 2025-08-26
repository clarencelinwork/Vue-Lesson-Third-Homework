<script setup>

const props = defineProps({
  cartItems: { type: Array, required: true },
  selectCount: { type: Array, required: true },
  cartTotal: { type: Number, required: true },
})

const emit = defineEmits(['emit-item-count','emit-remove-item'])
const passItemCount = (itemCount) => {
  emit('emit-item-count', itemCount)
}

const processRemoveItem = (cartItemId) => {
  emit('emit-remove-item', cartItemId)
}

function getItemTotal(cartItem) {
  return cartItem.price * cartItem.count
}
</script>

<template>
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
    <tr v-for="cartItem in props.cartItems" :key="cartItem.id">
      <td>
        <button type="button" @click="processRemoveItem(cartItem.id)">X</button>
      </td>
      <td>{{ cartItem.name }}</td>
      <td>{{ cartItem.description }}</td>
      <td>
        <select
          class="form-control"
          @change="passItemCount(cartItem)"
          v-model="props.selectCount[cartItem.id]"
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
      <td class="total-font" colspan="6">總計:{{ props.cartTotal }}</td>
    </tr>
    </tfoot>
  </table>
</template>

<style scoped>

</style>

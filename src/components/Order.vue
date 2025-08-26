<script setup>
const props = defineProps({
  order: { type: Array, required: true },
  orderRemark: { type: String },
  orderTotal: { type: Number, required: true },
})
</script>

<template>
  <div class="card p-1">
    <table class="table">
      <thead>
        <tr>
          <th>品項</th>
          <th>數量</th>
          <th>小記</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="orderItem in props.order" :key="orderItem.id">
          <td>{{ orderItem.name }}</td>
          <td>{{ orderItem.count }}</td>
          <td>{{ orderItem.total }}</td>
        </tr>
      </tbody>
    </table>
    <p class="text-right" v-if="orderRemark">備註:{{ props.orderRemark }}</p>
    <p class="text-right total-font">總計:${{ orderTotal }}</p>
  </div>
</template>

<style scoped>
.total-font {
  font-size: 22px;
}

.table thead tr:first-child th {
  border-top: 0;
}
</style>

<script setup>
import { ref } from 'vue'

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

const emit = defineEmits(['emit-drink'])
const pass = (drink) => {
  emit('emit-drink', drink)
}
</script>

<template>
  <div
    class="drink-list mouse-pointer"
    v-for="drink in drinks"
    :key="drink.id"
    @click="pass(drink)"
  >
    <div class="name-font">{{ drink.name }} ${{ drink.price }}</div>
    <div>{{ drink.description }}</div>
  </div>
</template>

<style scoped>
.drink-list {
  border: 1px solid #000;
  border-radius: 5px;
  padding: 10px;
}

.mouse-pointer {
  cursor: pointer;
}
</style>

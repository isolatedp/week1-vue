<template>
  <h1>第一週作業</h1>
  <table>
    <thead>
      <tr>
        <th scope="col" style="text-align: start">品項</th>
        <th scope="col" style="text-align: start">描述</th>
        <th scope="col" style="text-align: start">價格</th>
        <th scope="col" style="text-align: start">庫存</th>
        <th scope="col" style="text-align: start">功能</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="product in products" :key="product.id">
        <td>
          <input
            v-if="isEdit && editProduct && editProduct.id === product.id"
            v-model="editProduct.name"
            type="text"
          />
          <span v-else>{{ product.name }}</span>
        </td>
        <td>
          <input
            v-if="isEdit && editProduct && editProduct.id === product.id"
            v-model="product.description"
            type="text"
          />
          <span v-else>{{ product.description }}</span>
        </td>
        <td>{{ product.price }}</td>
        <td>
          <button v-on:click="reduceInventory(product.id)">&ensp;-&ensp;</button>
          {{ product.inventory }}
          <button v-on:click="addInventory(product.id)">&ensp;+&ensp;</button>
        </td>
        <td>
          <button v-if="!isEdit" @click="editProductHandle(product.id)">修改</button>
          <span v-else-if="isEdit && editProduct && editProduct.id === product.id">
            <button @click="confirmEdit">確定</button>&ensp;
            <button @click="cancelEdit">取消</button>
          </span>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Product {
  id: number
  name: string
  description: string
  price: number
  inventory: number
}

// 商品資料
const products = ref([
  { id: 1, name: '珍珠奶茶', description: '香濃奶茶搭配QQ珍珠', price: 50, inventory: 20 },
  { id: 2, name: '冬瓜檸檬', description: '清新冬瓜配上新鮮檸檬', price: 45, inventory: 18 },
  { id: 3, name: '翡翠檸檬', description: '綠茶與檸檬的完美結合', price: 55, inventory: 34 },
  { id: 4, name: '四季春茶', description: '香醇四季春茶，回甘無比', price: 45, inventory: 10 },
  { id: 5, name: '阿薩姆奶茶', description: '阿薩姆紅茶搭配香醇鮮奶', price: 50, inventory: 25 },
  { id: 6, name: '檸檬冰茶', description: '檸檬與冰茶的清新組合', price: 45, inventory: 20 },
  { id: 7, name: '芒果綠茶', description: '芒果與綠茶的獨特風味', price: 55, inventory: 18 },
  { id: 8, name: '抹茶拿鐵', description: '抹茶與鮮奶的絕配', price: 60, inventory: 20 }
])

const isEdit = ref(false)
const editOriginal = ref<Product | null>(null)
const editProduct = ref<Product | null>(null)

// 增加庫存
function addInventory(pid: number) {
  // 增加庫存
  products.value[pid - 1].inventory++
}

// 減少庫存
function reduceInventory(pid: number) {
  // 減少庫存
  if (products.value[pid - 1].inventory === 0) {
    alert('庫存不足，無法減少')
    return
  }
  products.value[pid - 1].inventory--
}

// 編輯商品
function editProductHandle(pid: number) {
  isEdit.value = true
  editProduct.value = products.value[pid - 1]
  editOriginal.value = { ...editProduct.value }
}

function cancelEdit() {
  if (!editOriginal.value || !editProduct.value) return
  isEdit.value = false
  products.value[editProduct.value.id - 1] = editOriginal.value
  editProduct.value = null
  editOriginal.value = null
}

function confirmEdit() {
  if (!editProduct.value) return
  isEdit.value = false
  products.value[editProduct.value.id - 1] = editProduct.value
  editProduct.value = null
  editOriginal.value = null
}
</script>

<style scoped></style>

<template>
    <div class="pc-order">
        <!-- 左侧菜单 -->
        <div class="food-section">
            <h2 class="title">菜单</h2>
            <div class="food-list">
                <div v-for="food in foodList" :key="food.id" class="food-item">
                    <div class="food-name">{{ food.name }}</div>
                    <div class="food-price">￥{{ food.price }}</div>
                    <button class="add-btn" @click="addToCart(food)">加入购物车</button>
                </div>
            </div>
        </div>

        <!-- 右侧购物车 -->
        <div class="cart-section">
            <h2 class="cart-title">购物车</h2>
            <div v-if="cartList.length === 0" class="cart-empty">
                购物车空空如也，快去添加菜品吧！
            </div>
            <div v-else>
                <div v-for="item in cartList" :key="item.id" class="cart-item">
                    <div>{{ item.name }} x {{ item.quantity }}</div>
                    <div>￥{{ item.price * item.quantity }}</div>
                </div>
                <div class="total-price">总价: ￥{{ getTotalPrice() }}</div>
                <button class="pay-btn" @click="pay">去支付</button>
            </div>
        </div>

        <!-- 支付弹窗 -->
        <div v-if="showPayBox" class="pay-mask">
            <div class="pay-box">
                <h3>支付成功！</h3>
                <div class="pay-tip">感谢您的购买，祝您用餐愉快！</div>
                <button class="close-btn" @click="closePayBox">关闭</button>
            </div>
        </div>

        <div class="container">
            <p>侧边栏</p>
            <li v-for="list in mylist" :key="list.id">
                {{ list.name }} {{ list.price }}
            </li>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const foodList = ref([
    { id: 1, name: "宫保鸡丁", price: 28 },
    { id: 2, name: "麻婆豆腐", price: 18 },
    { id: 3, name: "鱼香肉丝", price: 22 },
    { id: 4, name: "红烧肉", price: 35 },
    { id: 5, name: "清炒时蔬", price: 15 },
    { id: 6, name: "酸辣汤", price: 12 },
    { id: 7, name: "蛋炒饭", price: 20 },
    { id: 8, name: "牛肉面", price: 25 }
])

const mylist = ref([
    { id: 1, name: "Home" },
    { id: 2, name: "Message" },
    { id: 3, name: "App" },
    { id: 4, name: "Name" },
    { id: 5, name: "navigation" },
    { id: 6, name: "search" },
    { id: 7, name: "setting" },
    { id: 8, name: "user" }
])

const cartList = ref([])
const showPayBox = ref(false)

function addToCart(food) {
    const existingItem = cartList.value.find(item => item.id === food.id)
    if (existingItem) {
        existingItem.quantity++
    } else {
        cartList.value.push({ ...food, quantity: 1 })
    }
}

function getTotalPrice() {
    return cartList.value.reduce((total, item) => total + item.price * item.quantity, 0)
}

function pay() {
    if (cartList.value.length === 0) {
        alert("请先添加菜品到购物车！")
        return
    }
    showPayBox.value = true
}

function closePayBox() {
    showPayBox.value = false
    cartList.value = []
}
</script>

<style scoped>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Microsoft YaHei", sans-serif;
}

body {
    background: #f5f5f5;
}

.pc-order {
    display: flex;
    max-width: 1200px;
    margin: 30px auto;
    background: #fff;
    border-radius: 10px;
    box-shadow: 0 0 10px #ccc;
    overflow: hidden;
}

/* 左侧菜单 */
.food-section {
    width: 75%;
    padding: 20px;
    float: right;
}

.title {
    font-size: 24px;
    color: #ff5722;
    margin-bottom: 20px;
    border-left: 5px solid #ff5722;
    padding-left: 10px;
}

.food-list {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 15px;
}

.food-item {
    border: 1px solid #eee;
    border-radius: 8px;
    padding: 15px;
    text-align: center;
    background: #fafafa;
    transition: 0.2s;
}

.food-item:hover {
    transform: translateY(-3px);
    box-shadow: 0 2px 8px #ddd;
}

.food-name {
    font-weight: bold;
    margin: 8px 0;
}

.food-price {
    color: #f56c00;
    font-size: 16px;
    margin-bottom: 10px;
}

.add-btn {
    background: #ff5722;
    color: #fff;
    border: none;
    padding: 6px 12px;
    border-radius: 4px;
    cursor: pointer;
}

/* 右侧购物车 */
.cart-section {
    width: 25%;
    background: #2a2a2a;
    color: #fff;
    padding: 20px;
}

.cart-title {
    font-size: 18px;
    margin-bottom: 15px;
    border-bottom: 1px solid #555;
    padding-bottom: 10px;
}

.cart-empty {
    color: #999;
    text-align: center;
    margin-top: 30px;
}

.cart-item {
    display: flex;
    justify-content: space-between;
    padding: 8px 0;
    border-bottom: 1px dashed #555;
}

.total-price {
    margin-top: 20px;
    font-size: 18px;
    color: #ff9800;
    font-weight: bold;
    text-align: right;
}

.pay-btn {
    width: 100%;
    background: #07c160;
    color: white;
    padding: 12px;
    border: none;
    border-radius: 6px;
    font-size: 16px;
    margin-top: 15px;
    cursor: pointer;
}

/* 支付弹窗 */
.pay-mask {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
}

.pay-box {
    background: #fff;
    width: 400px;
    padding: 30px;
    border-radius: 10px;
    text-align: center;
}

.pay-box h3 {
    margin-bottom: 15px;
    color: #333;
}

.pay-tip {
    margin: 10px 0;
    font-size: 16px;
}

.close-btn {
    background: #ff5722;
    color: #fff;
    border: none;
    padding: 8px 20px;
    border-radius: 4px;
    margin-top: 15px;
    cursor: pointer;
}

.container {
    height: 1000px;
    width: 193px;
    background: #eee;
}

li {
    list-style: none;
    background-color: #ccc;
    margin: 10px;
    padding: 10px;
    line-height: 10px;
    color: #ff9800;
}
li:hover {
    background-color: #ff5722;
    color: #fff;
}
</style>

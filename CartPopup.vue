<template>
    <div class="cart-container">
        <button @click="toggleCartPopup" class="cart-button">
         Корзина недвижимости (<span>{{ cartCount }}</span>)
        </button>
        <div v-if="showCartPopup" class="cart-popup">
         <h2 class="cart-title">Корзина</h2>
         <ul class="cart-items">
            <li v-for="item in cart" :key="item.name">
             {{ item.name }} - ${{ item.price }} x {{ item.quantity }}
             <button @click="removeFromCart(item)" class="remove-button">Удалить</button>
            </li>
         </ul>
         <p class="cart-total">Total: ${{ totalPrice }}</p>
         <button @click="checkout" class="checkout-button">Купить</button>
        </div>
    </div>
    </template>
    
    <script>
    export default {
    props: {
        cart: Array,
    },
    data() {
        return {
         showCartPopup: false,
        }
    },
    computed: {
        cartCount() {
         return this.cart.reduce((acc, item) => acc + item.quantity, 0)
        },
        totalPrice() {
         return this.cart.reduce((acc, item) => acc + item.price * item.quantity, 0).toFixed(2)
        },
    },
    methods: {
        toggleCartPopup() {
         this.showCartPopup = !this.showCartPopup
        },
        removeFromCart(item) {
         this.$emit('remove-from-cart', item)
        },
        checkout() {
         alert('Order placed successfully!')
         this.$emit('checkout')
         this.showCartPopup = false
        },
    },
    }
    </script>
    
    <style scoped>
    .cart-container {
    position: fixed;
    top: 16px;
    right: 16px;
    }
    .cart-button {
    background-color: blue;
    color: white;
    padding: 8px 16px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    }
    .cart-popup {
    background-color: white;
    padding: 16px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-top: 8px;
    }
    .cart-title {
    font-size: 1.5rem;
    font-weight: bold;
    margin-bottom: 16px;
    }
    .cart-items {
    list-style-type: disc;
    padding-left: 20px;
    margin-bottom: 16px;
    }
    .cart-total {
    font-size: 1.25rem;
    font-weight: bold;
    margin-bottom: 16px;
    }
    .checkout-button {
    background-color: green;
    color: white;
    padding: 8px 16px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    }
    .remove-button {
    background-color: red;
    color: white;
    padding: 4px 8px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    margin-left: 8px;
    }
    </style>
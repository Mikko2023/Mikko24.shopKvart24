<template>
    <div class="product-card" @click="openPopup">
        <img :src="product.images[0]" :alt="product.name" class="product-image" />
        <h3 class="product-name">{{ product.name }}</h3>
        <p class="product-price">${{ product.price }}</p>
        <button class="view-product">View</button>
    </div>
    </template>
    
    <script>
    export default {
    props: {
        product: Object,
    },
    methods: {
        openPopup() {
         this.$emit('view-product', this.product)
        },
    },
    }
    </script>
    
    <style scoped>
    .product-card {
    background-color: white;
    padding: 16px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s;
    cursor: pointer;
    }
    .product-card:hover {
    transform: scale(1.05);
    }
    .product-image {
    width: 100%;
    height: 150px;
    object-fit: cover;
    margin-bottom: 8px;
    }
    .product-name {
    font-size: 1.25rem;
    font-weight: bold;
    margin-bottom: 8px;
    }
    .product-price {
    color: gray;
    margin-bottom: 8px;
    }
    .view-product {
    background-color: blue;
    color: white;
    padding: 8px 16px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    }
    </style>
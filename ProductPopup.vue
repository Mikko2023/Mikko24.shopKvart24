<template>
<div v-if="product" class="popup">
    <div class="popup-content">
     <button @click="closePopup" class="close-button">Закрыть</button>
     <div class="images-container">
        <img
         v-for="image in product.images"
         :key="image"
         :src="image"
         alt="Product Image"
         class="popup-image"
         @mouseover="zoomIn"
         @mouseout="zoomOut"
        />
     </div>
     <h3 class="popup-product-name">{{ product.name }}</h3>
     <p class="popup-product-price">${{ product.price }}</p>
     <p class="popup-product-description">{{ product.description }}</p>
     <label for="quantity" class="quantity-label">Quantity:</label>
     <input type="number" v-model="quantity" class="quantity-input" min="1" />
     <button @click="addToCart" class="add-to-cart-button">Добавить в корзину</button>
    </div>
</div>
</template>

<script>
export default {
props: {
    product: Object,
},
data() {
    return {
     quantity: 1,
    }
},
methods: {
    closePopup() {
     this.$emit('close')
    },
    addToCart() {
     this.$emit('add-to-cart', { ...this.product, quantity: this.quantity })
     this.closePopup()
    },
    zoomIn(event) {
     event.target.style.transform = 'scale(4)';
     event.target.style.zIndex = '1000';
    },
    zoomOut(event) {
     event.target.style.transform = 'scale(1)';
     event.target.style.zIndex = '1';
    },
},
}
</script>

<style scoped>
.popup {
position: fixed;
top: 0;
left: 0;
right: 0;
bottom: 0;
display: flex;
align-items: center;
justify-content: center;
background-color: rgba(0, 0, 0, 0.5);
}
.popup-content {
background-color: white;
padding: 16px;
border-radius: 8px;
max-width: 500px;
width: 100%;
}
.close-button {
background-color: red;
color: white;
padding: 8px 16px;
border: none;
border-radius: 4px;
cursor: pointer;
margin-bottom: 16px;
}
.images-container {
display: flex;
gap: 8px;
margin-bottom: 16px;
}
.popup-image {
width: 100px;
height: 100px;
object-fit: cover;
transition: transform 0.2s;
}
.popup-product-name {
font-size: 1.25rem;
font-weight: bold;
margin-bottom: 8px;
}
.popup-product-price {
color: gray;
margin-bottom: 8px;
}
.popup-product-description {
margin-bottom: 16px;
}
.quantity-label {
display: block;
margin-bottom: 8px;
}
.quantity-input {
width: 100%;
padding: 8px;
margin-bottom: 16px;
}
.add-to-cart-button {
background-color: green;
color: white;
padding: 8px 16px;
border: none;
border-radius: 4px;
cursor: pointer;
}
</style>
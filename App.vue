<template>
    <div>
        <header class="header">
         <h1 class="title">Магазин квартир</h1>
        </header>
        <main class="main">
         <div class="product-grid">
            <ProductCard
             v-for="product in products"
             :key="product.id"
             :product="product"
             @view-product="viewProduct"
            />
         </div>
         <ProductPopup
            v-if="showProductPopup"
            :product="selectedProduct"
            @close="closeProductPopup"
            @add-to-cart="addToCart"
         />
         <CartPopup :cart="cart" @checkout="checkout" @remove-from-cart="removeFromCart" />
        </main>
    </div>
    </template>
    
    <script>
    import { ref } from 'vue'
    import ProductCard from './components/ProductCard.vue'
    import ProductPopup from './components/ProductPopup.vue'
    import CartPopup from './components/CartPopup.vue'
    
    export default {
    components: {
        ProductCard,
        ProductPopup,
        CartPopup,
    },
    setup() {
        const products = ref([
         { id: 1, name: 'Квартира в г.Паттайя - Тайланд', price: 10, description: 'Данный комплекс отличается современным и стильным дизайном, который сочетает в себе эстетику и функциональность. Архитектурные решения направлены на создание комфортных и роскошных жилых пространств. Каждая из квартир Grand Solaire Noble оборудована высококачественной импортной сантехникой и оборудованием. В стандартную комплектацию входят элегантные черные кухни европейского стандарта, роскошная мраморная плитка, электронные туалетные системы, большие зеркала со светодиодной подсветкой и энергоэффективный кондиционер премиум-класса и полная меблировка.', images: ['https://optim.tildacdn.one/tild3631-3634-4830-b537-366665613034/-/resize/560x/-/format/webp/imagemiddle2_1.png', 'https://optim.tildacdn.one/tild6633-3530-4239-a165-633137303432/-/resize/560x/-/format/webp/f04fe9b0-74e3-4a4a-b.jpeg', 'https://optim.tildacdn.one/tild3465-3662-4963-b363-383535636361/-/resize/560x/-/format/webp/sfqbvhff.jpeg', 'https://optim.tildacdn.one/tild3663-6362-4535-b938-373163363933/-/resize/518x/-/format/webp/SA.png'] },
         { id: 2, name: 'Квартира в г.Мурманске - Россия', price: 20, description: 'Мурманская область, Мурманск, Ленинский, просп. Героев-Североморцев, 9к1.Из окна открывается прекрасный панорамный вид на Семеновское озеро, памятник Алеше, Храм Спас На Водах, детский городок, Кольский залив.В квартире новые счетчики, замена одного окна, свежие обои в маленькой комнате. Вторая комната подготовлена к поклейке.Крыша не течет.Купив нашу квартиру по привлекательной цене, вы сможете воплотить в жизнь все ваши мечты, создав идеальное место для комфортного проживания.', images: ['https://images.cdn-cian.ru/images/2188954312-1.jpg', 'https://images.cdn-cian.ru/images/2188954316-1.jpg', 'https://images.cdn-cian.ru/images/2188954322-1.jpg', 'https://images.cdn-cian.ru/images/2171510740-1.jpg'] },
         { id: 3, name: 'Апартаменты в Торревьехе - Испания', price: 30, description: 'Основные характеристики: Цена продажи 212 000 €, Площадь-70м2,Всего комнат-4,Количество спален-3,Количество ванных 1,Стадия строительства-Вторичная недвижимость, Адрес Испания, Торревьеха,', images: ['https://img.prian.ru/2024_06/7/20240607043930370358759o.jpg', 'https://img.prian.ru/2024_06/7/202406070439441573507642o.jpg', 'https://img.prian.ru/2024_06/7/20240607043957516435453o.jpg', 'https://img.prian.ru/2024_06/7/202406070439341439409824o.jpg'] },
         { id: 4, name: 'Квартиры и пентхаусы В ADMIRAL PREMIUM RESIDENCE В КОНАКЛЫ, Алания - Турция', price: 40, description: 'Апартаменты, пентхаусы, виллы каскадом расположились на горе с панорамным видом на Средиземное море. 10 малоэтажных блоков, 123 апартамента, 7 вилл.Лучшие архитекторы и дизайнеры Турции создали для Вас новый мир стиля, элитарности и комфорта.', images: ['http://bravoinvest.ru/media/catalog/itemimage/a_c4ebe0c1ca7bcc9900105b703bae5e42_thm_wtm_1600x1200.JPG', 'http://bravoinvest.ru/media/catalog/itemimage/a_5c93d4f7d074da466833464cdd59f119_thm_fix_150x150.jpg', 'http://bravoinvest.ru/media/catalog/itemimage/a_5c09524307cd21eff702d28a942963c5_thm_fix_150x150.JPG'] },
         { id: 5, name: 'Квартира в г.Мончегорске, Мурманская область - Россия', price: 50, description: '2-комн. квартира, 54,8 м² Если вы мечтаете о собственной квартире с потрясающим видом на горы, то эта недвижимость идеально подойдет для вас. Расположенная на девятом этаже, она предлагает не только великолепный пейзаж за окном, но и ряд других преимуществ', images: ['https://images.cdn-cian.ru/images/kvartira-monchegorsk-lesnaya-ulica-2200041383-1.jpg', 'https://images.cdn-cian.ru/images/kvartira-monchegorsk-lesnaya-ulica-2199920360-1.jpg', 'https://images.cdn-cian.ru/images/kvartira-monchegorsk-lesnaya-ulica-2200041330-1.jpg', 'https://images.cdn-cian.ru/images/kvartira-monchegorsk-lesnaya-ulica-2199920391-1.jpg'] },
         { id: 6, name: 'Квартира в Шарм-эль-Шейхе - Египет', price: 60, description: 'Просторная полностью меблированная квартира с 2-я спальнями общей площадью 90 кв. м расположена в престижном жилом комплексе на самом берегу моря.На территории комплекса несколько бассейнов, зона отдыха, рестораны, кафе, дайвинг центр, услуги по уборке квартир, управляющая компания, круглосуточная охрана. Собственный пляж комплекса отлично оборудован.', images: ['https://saveoffer.ru/storage/realestate/f5e4af30c402216fc9936caeeaa9e88a.png', 'https://saveoffer.ru/storage/realestate/dedcdc0e780777eb423fabe2dba6d48e.png', 'https://saveoffer.ru/storage/realestate/976019012f501f1fa51744453443f790.png', 'https://saveoffer.ru/storage/realestate/f7ca154cbd69c994d7fe6a828e029991.png',] },
         { id: 7, name: 'Квартира в Санкт-Петербурге, р-н Петроградский - Россия', price: 70, description: 'Продается 4-комн. квартира, 160 м² В одном из самых красивых зданий на  улице  Ленина —в  доходном доме Колобовых, предлагается к продаже эксклюзивная двухуровневая квартира.На первом этаже просторная гостиная с камином выходом на французский балкон, откуда открывается замечательный вид на город. Также есть круглый  кабинет с собственной библиотекой и старинным камином, который прекрасно сохранился со времён 1910-ых годов.', images: ['https://images.cdn-cian.ru/images/2154809230-1.jpg', 'https://images.cdn-cian.ru/images/2154809238-1.jpg', 'https://images.cdn-cian.ru/images/2154809255-1.jpg', 'https://images.cdn-cian.ru/images/2154809216-1.jpg'] },
         { id: 8, name: 'Квартира в г.Сочи Краснодарский край - Россия', price: 80, description: 'К Вашему вниманию элитная квартира в ЖК "Королевский Парк".Ремонт в квартире выполнен по индивидуальному дизайн проекту. Использования только качественных материалов гарантирует долгую службу.Данная квартира - отличная возможность для приобретения собственной резиденции в городе-курорте Сочи.', images: ['https://images.cdn-cian.ru/images/kvartira-sochi-kurortnyy-prospekt-2065884078-1.jpg', 'https://images.cdn-cian.ru/images/kvartira-sochi-kurortnyy-prospekt-2065884064-1.jpg', 'https://images.cdn-cian.ru/images/kvartira-sochi-kurortnyy-prospekt-2065884069-1.jpg', 'https://images.cdn-cian.ru/images/kvartira-sochi-kurortnyy-prospekt-2065884068-1.jpg'] }
        ])

    
        const showProductPopup = ref(false)
        const selectedProduct = ref({})
        const cart = ref([])
    
        const viewProduct = (product) => {
         selectedProduct.value = product
         showProductPopup.value = true
        }
    
        const closeProductPopup = () => {
         showProductPopup.value = false
        }
    
        const addToCart = (product) => {
         const existingProduct = cart.value.find(item => item.name === product.name)
         if (existingProduct) {
            existingProduct.quantity += product.quantity
         } else {
            cart.value.push(product)
         }
        }
    
        const removeFromCart = (product) => {
         cart.value = cart.value.filter(item => item.name !== product.name)
        }
    
        const checkout = () => {
         cart.value = []
        }
    
        return {
         products,
         showProductPopup,
         selectedProduct,
         cart,
         viewProduct,
         closeProductPopup,
         addToCart,
         removeFromCart,
         checkout,
        }
    },
    }
    </script>
    
    <style scoped>
    .header {
    text-align: center;
    padding: 16px;
    }
    .title {
    font-size: 2rem;
    font-weight: bold;
    }
    .main {
    padding: 16px;
    }
    .product-grid {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    gap: 16px;
    }
    @media (min-width: 640px) {
    .product-grid {
        grid-template-columns: repeat(2, 1fr);
    }
    }
    @media (min-width: 768px) {
    .product-grid {
        grid-template-columns: repeat(4, 1fr);
    }
    }
    </style>
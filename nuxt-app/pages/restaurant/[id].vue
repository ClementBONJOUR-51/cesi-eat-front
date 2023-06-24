<template>
    <div>
        <composants_generiqueNavBar class="sticky"/>
        <div class="flex justify-center items-start flex-col md:flex-row md:justify-between md:items-start flex-wrap h-screen">
            
            <div class="md:w-3/4 overflow-auto">
                <h1 class="bg-emerald-600 text-white text-xl md:text-2xl p-2 text-center md:text-left">Restaurant -
                    <b>{{ restaurant.restorant_name }}</b>
                </h1>
                <div v-if="isLoading">Loading...</div>
                <div v-else class="border-0 border-r-2">
                    <h2 class="bg-emerald-800 text-white text-xl md:text-1xl p-1 text-center md:text-left">Menu</h2>
                    <div class="flex flex-wrap p-2">
                        <Menu v-for="menu in menus" :key="menu._id" :menu="menu" @add-to-cart="addToCart" />
                    </div>
                    <h2 class="bg-emerald-800 text-white text-xl md:text-1xl p-1 text-center md:text-left">Produits</h2>
                    <div class="flex flex-wrap p-2">
                        <Product v-for="product in products" :key="product._id" :product="product" @add-to-cart="addToCart" />
                    </div>
                </div>
            </div>
            <div class="md:w-1/4 flex flex-col bg-slate-50">
                <Cart :cart="cart" @remove-from-cart="removeFromCart" />
                <OrderForm :order="order" :restaurant="restaurant" :cart="cart" />
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import jwt_decode from "jwt-decode";
import Product from '../../components/components_customers/Product.vue';
import Cart from '../../components/components_customers/Cart.vue';
import OrderForm from '../../components/components_customers/OrderForm.vue';
import Menu from '../../components/components_customers/Menu.vue';


export default {
    components: { Product, Cart, OrderForm, Menu },
    data() {
        return {
            isLoading: true,
            restaurant: {},
            products: [],
            menus: [],
            cart: [],
            order: {
                restorant: "",
                order_state: "En cours",
                paid: 0,
                products: [],
                customer: {
                    id_customer: "",
                    firstname: "",
                    lastname: "",
                    gender: "",
                    birthday: "",
                    phone: "",
                    email: ""
                },
                address: {
                    street: "",
                    postal_code: "",
                    city: "",
                    street_number: null,
                    lati: 0,
                    longi: 0
                },
                discount: 5,
            },
        };
    },
    methods: {
        addToCart(product) {
            this.cart.push(product);
        },
        fetchMenus() {
            const idResto = this.$route.params.id;

            axios.get(`http://localhost:3000/getAllMenusByRestorant/${idResto}`)
                .then(response => {
                    this.menus = response.data.result.menus;
                })
                .catch(error => {
                    console.error("Erreur lors de la récupération des menus:", error);
                });
        },
        fetchProducts() {
            const idResto = this.$route.params.id;
            axios.get(`http://localhost:3000/getAllProductsFromRestorant/${idResto}`)
                .then(response => {
                    this.products = response.data.result.products;
                    this.restaurant = response.data.result.products[0].restorant;
                })
                .catch(error => {
                    console.error("Erreur lors de la récupération des produits:", error);
                });

        },
        removeFromCart(index) {
            this.cart.splice(index, 1); // Supprimer le produit du panier en utilisant l'index
        }
    },
    async mounted() {
        try {
            const token = localStorage.getItem('authToken');
            if (token) {
                const decodedToken = jwt_decode(token);
                console.log(decodedToken);
                this.order.customer = {
                    id_customer: decodedToken.id,
                    firstname: decodedToken.firstname,
                    lastname: decodedToken.lastname,
                    gender: decodedToken.gender,
                    birthday: decodedToken.birthday,
                    phone: decodedToken.phone,
                    email: decodedToken.email
                };
            } else {
                alert("TOKEN NOT FOUND")
            }
            this.fetchMenus();
            this.fetchProducts();
            this.isLoading = false;
        } catch (error) {
            console.error(error);
        }
    },
};
</script>

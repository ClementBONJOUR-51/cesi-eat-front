<template>
    <div>
        <composants_generiqueNavBar class="sticky"/>
        
        <composants_generiqueSearchBar class="md:hidden"/>
        <div class="flex justify-center items-start flex-col md:flex-row md:justify-between md:items-start flex-wrap h-screen ">
            <div class="md:w-3/4 overflow-auto pb-32">
                <h1 class="bg-emerald-600 text-white text-xl md:text-2xl p-2 text-center md:text-left">Restaurant -
                    <b>{{ restaurant.restorant_name }}</b>
                </h1>
                <div v-if="isLoading">Loading...</div>
                <div v-else class="border-0 border-r-2">
                    <h2 class="bg-emerald-800 text-white text-xl md:text-1xl p-1 text-center md:text-left">Menu</h2>
                    <div class="flex flex-wrap p-2 justify-center">
                        <Menu v-for="menu in menus" :key="menu._id" :menu="menu" @add-to-cart="addToCart" />
                    </div>
                    <h2 class="bg-emerald-800 text-white text-xl md:text-1xl p-1 text-center md:text-left">Produits</h2>
                    <div class="flex flex-wrap p-2 justify-center">
                        <Product v-for="product in products" :key="product._id" :product="product" @add-to-cart="addToCart" />
                    </div>
                </div>
            </div>
            <transition name="slide">
                <div v-if="showCart" class="fixed bottom-0 inset-x-0 md:hidden  md:pb-0">
                    <div class="max-w-3xl mx-auto w-full px-4 sm:px-6">
                        <div class="rounded-lg bg-white shadow-lg overflow-hidden">
                            <div class="p-4">
                                <div class="flex items-center justify-between">
                                    <h2 class="text-lg font-medium text-gray-900">Your Cart</h2>
                                    <button @click="showCart = false" class="ml-2 rounded-md p-2 inline-flex items-center justify-center text-gray-400 hover:text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-white focus:ring-gray-900">
                                        <span class="sr-only">Close panel</span>
                                        <!-- Heroicon name: x -->
                                        <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                                        </svg>
                                    </button>
                                </div>
                                <!-- Cart -->
                                <Cart :cart="cart" @remove-from-cart="removeFromCart" />
                                <OrderForm :order="order" :restaurant="restaurant" :cart="cart" />
                            </div>
                        </div>
                    </div>
                </div>
            </transition>

            <!-- Panier desktop -->
            <div class="md:w-1/4 hidden md:flex flex-col bg-slate-50">
                <Cart :cart="cart" @remove-from-cart="removeFromCart" />
                <OrderForm :order="order" :restaurant="restaurant" :cart="cart" />
            </div>
        </div>

        <!-- Bouton mobile pour afficher le panier -->
        <div class="fixed inset-x-0 bottom-16 p-2 md:hidden">
            <div class="max-w-3xl mx-auto w-full px-4">
                <div class="rounded-lg shadow-md bg-white p-4 flex items-center justify-between">
                    <h2 class="font-semibold text-gray-700">Mon Panier</h2>
                    <button @click="showCart = true" class="rounded-md border border-transparent px-3 py-2 text-sm font-medium text-white bg-emerald-600 hover:bg-emerald-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-white focus:ring-emerald-500">
                        View
                    </button>
                </div>
            </div>
        </div>
        <composants_generiqueMobileNavBar class="md:hidden"/>
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
            showCart: false,
            isLoading: true,
            restaurant: {},
            products: [],
            menus: [],
            cart: [],
            order: {
                restorant: "",
                order_state: "CREATED",
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

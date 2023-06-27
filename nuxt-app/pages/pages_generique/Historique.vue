<template>
    <div class="bg-emerald-400 h-screen flex flex-col justify-end">
        <div class="flex items-left justify-center h-[25%] flex-col">
            <div class="m-5">
                <composants_generiquePrevButton/>
            </div>
            <span class="text-2xl m-5">Mes Commandes</span>
        </div>
        <div class="bg-white flex-grow-0 h-[75%] rounded-t-[20px]">
            <div class="gap-1 flex flex-col items-center justify-center">
                <components_customersCard-history 
                v-bind:key="order._id" 
                v-for="order in userOrders.result.orders" 
                :order="order"/>
            </div> 
        </div>
        <composants_generiqueMobileNavBar class="md:hidden"/>
    </div>
</template>

<script>
    import axios from 'axios';
    import jwt_decode from "jwt-decode";
export default {

    data() {
        return {
            userOrders: {
                result: {
                    orders: [],
                },
                status: "",
            },
        }
    },
    async created() {
        const token = localStorage.getItem('authToken');
        const decoded = jwt_decode(token);
        const id_customer = decoded.id;
        console.log(id_customer);
        try {
            const response = await axios.get(`http://localhost:3000/getOrdersWithProductsAndRestorantsByCustomerId/${id_customer}`);
            this.userOrders = response.data;
            console.log(this.userOrders);
        } catch (error) {
            console.error(error);
        }
    },
    methods: {
        checkAuthentication() {
            
            const token = localStorage.getItem('authToken');
            console.log(token);
            if (token) {
                
            } else {
                this.redirectToLoginPage();
            }
        },
        redirectToLoginPage() {
            this.$router.push({ path: './pages_connexion/connexion' });
        }
    },
    mounted() {
        this.checkAuthentication();
    }
}
</script>
<template>
    <div>
        <form @submit.prevent="submitOrder">
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 mt-4 p-2">
                <div>
                    <label class="block">Numéro de rue:</label>
                    <input v-model="order.address.street_number" type="number" required
                        class="w-full mt-1 border border-black" />
                </div>
                <div>
                    <label class="block">Rue:</label>
                    <input v-model="order.address.street" type="text" required class="w-full mt-1 border border-black" />
                </div>
                <div>
                    <label class="block">Code Postal:</label>
                    <input v-model="order.address.postal_code" type="text" required
                        class="w-full mt-1 border border-black" />
                </div>
                <div>
                    <label class="block">Ville:</label>
                    <input v-model="order.address.city" type="text" required class="w-full mt-1 border border-black" />
                </div>
            </div>
            <div class="w-full p-2">
                <button type="submit" class="bg-emerald-500 hover:bg-emerald-700 text-white font-bold p-2 rounded w-full">
                    Commander
                </button>
            </div>

        </form>
    </div>
</template>


<script>
import axios from 'axios';
export default {
    props: ['order', 'restaurant', 'cart'],
    methods: {
        async submitOrder() {
            this.order.restorant = this.restaurant._id;
            this.order.products = this.cart.map((product) => ({ id_product: product._id }));

            try {
                const response = await axios.post("http://localhost:3000/createOrder", this.order);
                console.log(response.data);
                alert("Commande effectuée avec succès !")
                this.$router.push('/Acceuil')
            } catch (error) {
                console.error(error);
                alert("Erreur lors de la commande !")
            }
        },
    },
};
</script>

<template>
    <div class="grid gap-6 md:grid-cols-3 xl:grid-cols-3 auto-rows-fr">
        <div class="bg-emerald-400 rounded p-4">
            <h2 class="font-bold mb-2">Numéro de commande : {{ order.invoice_number }}</h2>
            <h2 class="font-bold mb-2">Destinataire</h2>
            <p class="mb-2">{{ order.customer.firstname }} {{ order.customer.lastname }}</p> <!-- Ou si pb mettre || '' -->
            <p class="mb-2">{{ order.customer.phone }}</p>
            <h2 class="font-bold mb-2">Adresse de livraison</h2>
            <p>{{ order.address.street_number }} {{ order.address.street }}</p>
            <p class="mb-2">{{ order.address.postal_code }} {{ order.address.city }}</p>
            <h2 class="font-bold mb-2">Restaurant</h2>
            <p>{{ order.restorant.restorant_name }}</p>
            <p class="mb-2">{{ order.restorant.phone_number }}</p>
            <p>{{ order.restorant.address.street_number }} {{ order.restorant.address.street }}</p>
            <p class="mb-2">{{ order.restorant.address.postal_code }} {{ order.restorant.address.city }}</p>
            <div class="flex justify-end mt-4">
                <button @click="deliveredOrder(order)" class="px-4 py-2 rounded bg-green-500 text-white mr-2">
                    Livrée
                </button>
            </div>
        </div>
    </div>
</template>
  
<script>
import axios from 'axios'
import jwt_decode from "jwt-decode";

export default {
    name: 'Card',
    data() {
        return {
            api_url: useRuntimeConfig().public.api_base_url
        }
    },
    props: {
        order: {
            type: Object,
            required: true
        },
        fetchData: {
            type: Function,
            required: true
        },
        removeOrder: {
            type: Function,
            required: true
        }
    },
    methods: {
        async deliveredOrder(order) {
            try {
                const response = await axios.put(`${this.api_url}/updateOrder/${order._id}`, {
                    restorant: order.restorant,
                    order_state: "DELIVERED",
                    paid: order.paid,
                    products: order.products,
                    customer: {
                        id_customer: order.customer.id_customer,
                        firstname: order.customer.firstname,
                        lastname: order.customer.lastname,
                        gender: order.customer.gender,
                        birthday: order.customer.birthday,
                        phone: order.customer.phone,
                        email: order.customer.email,
                    },
                    address: {
                        street: order.address.street,
                        postal_code: order.address.postal_code,
                        city: order.address.city,
                        street_number: order.address.street_number,
                        lati: order.address.lati,
                        longi: order.address.longi,
                    },
                    discount: order.discount,
                });

                if (response.data.status !== 'success') {
                    alert(response.data.message);
                } else {
                    // console.log(response.data.message);
                    this.removeOrder(order._id);
                }
            } catch (error) {
                console.error(error);
            }
        },
    }
}
</script>
  
  
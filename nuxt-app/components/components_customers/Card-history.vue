<template>
    <div class="bg-emerald-400 max-w-xs rounded-[15px] mt-5 shadow-lg">
        <div id="test" class="bg-emerald-400 rounded-[15px] mt-5 absolute top-0 right-0">
            <span>{{ order.date_in }}</span>
        </div>
        <img class="rounded-[15px] h-[80%] w-[100%]" src="https://picsum.photos/500/300">
        <div class="px-6 py-2 flex justify-between">
            <div class="font-bold text-l md:text-xl mb-2">{{ order.restorant.restorant_name }}</div>
        </div>
        <div class="px-6 py-2 flex justify-between">
            <div class="font-bold text-l md:text-xm mb-2">Numéro de commande : {{ order.invoice_number }}</div>
        </div>
        <div class="px-6 py-2">
            <div class="font-bold text-l md:text-xm mb-2 cursor-pointer flex justify-between items-center" @click="toggleDropdown">
                <span>Produits :</span>
                <svg class="w-5 h-5 transform transition-transform duration-200" :class="{'rotate-180': dropdownOpen}" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M10 12a2 2 0 100-4 2 2 0 000 4z" clip-rule="evenodd" />
                    <path fill-rule="evenodd" d="M4.293 6.293a1 1 0 011.414 0L10 10.586l4.293-4.293a1 1 0 111.414 1.414l-5 5a1 1 0 01-1.414 0l-5-5a1 1 0 010-1.414z" clip-rule="evenodd" />
                </svg>
            </div>
            <div class="overflow-hidden transition-max-height duration-200" :class="{'max-h-0': !dropdownOpen}">
                <div class="px-6 py-2 flex justify-between" v-for="product in order.products" :key="product._id">
                    <div class="font-bold text-l md:text-xm mb-2">{{ product.product_name }}</div>
                    <div class="font-bold text-l md:text-xm mb-2">{{ product.product_price }} €</div>
                </div>
            </div>
        </div>
        <div class="px-6 py-2">
            <div class="font-bold text-l md:text-xl mb-2">Prix Total : {{ calculateTotal() }} €</div>
        </div>

    </div>
</template>

<script>
export default {
    name: 'Card',
    props: {
        order: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            dropdownOpen: false
        }
    },
    methods: {
        calculateTotal() {
            let total = 0;
            for (let product of this.order.products) {
                total += product.product_price;
            }
            return total;
        },
        toggleDropdown() {
            this.dropdownOpen = !this.dropdownOpen;
        }
    }
    
}
</script>


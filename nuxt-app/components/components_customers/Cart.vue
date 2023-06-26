<template>
    <div>
        <h1 class="text-xl md:text-2xl p-2 bg-emerald-500 text-white text-center md:text-left">Panier</h1>
        <div id="panier" class="p-2 overflow-auto flex-1">
            <table class="w-full">
                <thead>
                    <tr>
                        <th class="font-bold text-sm md:text-lg">Produit</th>
                        <th class="font-bold text-sm md:text-lg">Prix</th>
                        <th>-</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(product, index) in cart" :key="index">
                        <td>{{ product.product_name }}</td>
                        <td>{{ product.product_price }} €</td>
                        <td>
                            <button @click="removeFromCart(index)" class="text-white bg-red-500 rounded px-2">
                                X
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
  
<script>
export default {
    props: ['cart'],
    methods: {
        removeFromCart(index) {
            this.$emit('remove-from-cart', index);
        },
    },
};
</script>
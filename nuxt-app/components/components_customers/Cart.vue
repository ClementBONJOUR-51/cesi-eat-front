<template>
    <div>
        <h1 class="text-xl md:text-2xl p-2 bg-emerald-500 text-white text-center md:text-left">Panier</h1>
        <div id="panier" class="p-2 overflow-auto flex-1">
            <div class="flex flex-col space-y-2">
                <div v-for="(product, index) in cart" :key="index" class="flex justify-between">
                    <div class="font-bold text-sm md:text-lg">
                        {{ product.product_name }}
                    </div>
                    <div class="text-sm md:text-lg">
                        {{ product.product_price }} €
                    </div>
                </div>
                <hr class="my-2">
                <div class="font-bold text-lg flex justify-between">
                    <div>Total:</div>
                    <div>{{ totalSum }} €</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['cart'],
    computed: {
        totalSum() {
            return this.cart.reduce((sum, product) => sum + product.product_price, 0);
        },
    },
};
</script>

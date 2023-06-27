<template>
    <div class="bg-white flex-grow-0 h-[75%] rounded-t-[20px] gap-1 flex flex-col items-center ">
        <div class="bg-emerald-400 max-w-xs rounded-[15px] mt-5 shadow-lg p-5">
            <div v-if="currentOrder" class="gap-1 flex flex-col items-center justify-center">
                <div class="text-2xl my-2">Information commande:</div>
                <div class="my-2 mb-2">
                    {{ currentOrder.order_state }}
                </div>
                <div class="text-2xl my-2">Etat de la commande:</div>
                <div class="my-2">
                    <div class="mb-2">
                        <span v-if="['Commande passée', 'En cours de préparation', 'En cours de livraison', 'Livré'].includes(currentOrder.order_state)">✔️</span>
                        <span v-else>❌</span> Commande passée
                    </div>
                    <div class="mb-2">
                        <span v-if="['En cours de préparation', 'En cours de livraison', 'Livré'].includes(currentOrder.order_state)">✔️</span>
                        <span v-else>❌</span> En cours de préparation
                    </div>
                    <div class="mb-2">
                        <span v-if="['En cours de livraison', 'Livré'].includes(currentOrder.order_state)">✔️</span>
                        <span v-else>❌</span> En cours de livraison
                    </div>
                    <div class="mb-2">
                        <span v-if="'Livré' === currentOrder.order_state">✔️</span>
                        <span v-else>❌</span> Livré
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    props: {
        currentOrder: {
            type: Object,
            required: true
        }
    },
}

</script>

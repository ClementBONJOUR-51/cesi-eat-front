<template>
    <!-- <div class="bg-emerald-400 md:bg-yellow-400  max-w-xs rounded-[15px] shadow-lg">
        <div class="px-6 py-2 flex justify-between">
            <div class="font-bold text-l md:text-xl mb-2">Invoice number :{{ order.invoice_number }}</div>
            <div class="font-bold text-l md:text-xl mb-2">{{ order.address.city }}({{ order.address.street }}+)</div>
        </div>
    </div> -->
    <div class="grid gap-6 md:grid-cols-3 xl:grid-cols-3 auto-rows-fr">
          <div class="bg-emerald-400 rounded p-4" >
            <h2 class="font-bold mb-2">{{ order.invoice_number }}</h2>
            <p class="mb-2">{{ order.address.city}} €</p>
            <p>{{ order.address.street }}</p>
            <div class="flex justify-end mt-4">
              <button @click="acceptOrder(product)" class="px-4 py-2 rounded bg-green-500 text-white mr-2">
                Accepter
              </button>
              <button @click="cancelOrder(product)" class="px-4 py-2 rounded bg-red-500 text-white">
                Annuler
              </button>
            </div>
          </div>
        </div>
</template>

<script>
 import axios from 'axios'
export default {
    name: 'Card',
    props: {
        order: {
            type: Object,
            required: true
        }
    }
}
</script>


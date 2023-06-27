<template>
    <div class="bg-emerald-400 max-w-xs rounded-[15px] shadow-lg mb-4 mr-4">
        <div class="px-0 py-0 grid grid-cols-1 gap-2">
            <div class="col-span-full">
                <img class="rounded-[15px] w-full h-32 object-cover" :src="'https://picsum.photos/300/200'" />
            </div>
            <div class="col-span-full font-bold text-l md:text-xl px-4 flex justify-between">
                <div class="flex items-center">
                    {{ menu.menu_name }}
                </div>
            </div>




            <div class="w-full px-4">
                <button @click="showModal = true"
                    class="bg-white hover:bg-emerald-600 hover:text-white font-bold mb-2 py-2 px-4 rounded-full w-full">Voir</button>
                <div v-if="showModal" class="fixed z-10 inset-0 overflow-y-auto" aria-labelledby="modal-title" role="dialog"
                    aria-modal="true">
                    <div class="flex items-center justify-center min-h-screen">
                        <div
                            class="inline-block align-middle bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all border-emerald-500 border-2 sm:my-8 sm:align-middle sm:max-w-lg sm:w-full shadow-2xl">
                            <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">

                                <div class="flex flex-col sm:flex-row sm:justify-between sm:items-center ">
                                    <label for="starters"
                                        class="block text-sm font-medium text-gray-700 sm:mr-4">Entrée</label>
                                    <select id="starters" v-model="selectedStarter"
                                        class="mt-1 sm:mt-0 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 sm:text-sm rounded-md border-emerald-500 border-2">
                                        <option v-for="starter in menu.menu_starters" :key="starter._id" :value="starter">
                                            {{ starter.product_name }} - {{ starter.product_price }}€
                                        </option>
                                    </select>
                                </div>

                                <div class="flex flex-col sm:flex-row sm:justify-between sm:items-center mt-4">
                                    <label for="dishes"
                                        class="block text-sm font-medium text-gray-700 sm:mr-4">Plats</label>
                                    <select id="dishes" v-model="selectedDish"
                                        class="mt-1 sm:mt-0 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 sm:text-sm rounded-md border-emerald-500 border-2">
                                        <option v-for="dish in menu.menu_dishes" :key="dish._id" :value="dish">
                                            {{ dish.product_name }} - {{ dish.product_price }}€
                                        </option>
                                    </select>
                                </div>

                                <div class="flex flex-col sm:flex-row sm:justify-between sm:items-center mt-4">
                                    <label for="desserts"
                                        class="block text-sm font-medium text-gray-700 sm:mr-4">Dessert</label>
                                    <select id="desserts" v-model="selectedDessert"
                                        class="mt-1 sm:mt-0 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 sm:text-sm rounded-md border-emerald-500 border-2">
                                        <option v-for="dessert in menu.menu_desserts" :key="dessert._id" :value="dessert">
                                            {{ dessert.product_name }} - {{ dessert.product_price }}€
                                        </option>
                                    </select>
                                </div>

                                <div class="flex flex-col sm:flex-row sm:justify-between sm:items-center mt-4">
                                    <label for="drinks"
                                        class="block text-sm font-medium text-gray-700 sm:mr-4">Boisson</label>
                                    <select id="drinks" v-model="selectedDrink"
                                        class="mt-1 sm:mt-0 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 sm:text-sm rounded-md border-emerald-500 border-2">
                                        <option v-for="drink in menu.menu_beverages" :key="drink._id" :value="drink">
                                            {{ drink.product_name }} - {{ drink.product_price }}€
                                        </option>
                                    </select>
                                </div>
                            </div>
                            <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
                                <button @click="addMenu"
                                    class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-blue-600 text-base font-medium text-white hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 sm:ml-3 sm:w-auto sm:text-sm">
                                    Ajouter
                                </button>
                                <button @click="showModal = false"
                                    class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">
                                    Fermer
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        menu: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            showModal: false,
            selectedStarter: null,
            selectedDish: null,
            selectedDessert: null,
            selectedDrink: null
        };
    },
    methods: {
        addMenu() {
            if (
                this.selectedStarter &&
                this.selectedDish &&
                this.selectedDessert &&
                this.selectedDrink
            ) {
                this.$emit("add-to-cart", this.selectedStarter);
                this.$emit("add-to-cart", this.selectedDish);
                this.$emit("add-to-cart", this.selectedDessert);
                this.$emit("add-to-cart", this.selectedDrink);

                this.showModal = false;
            } else {
                alert(
                    "Veuillez sélectionner une option pour chaque partie du menu avant d'ajouter au panier."
                );
            }
        }
    }
};
</script>
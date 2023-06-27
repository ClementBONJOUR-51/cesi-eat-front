<template>
    <h1 class="bg-emerald-600 text-white text-xl md:text-2xl p-2 text-center md:text-left">Stats</h1>
    <h2 class="bg-emerald-800 text-white text-xl md:text-1xl p-1 text-center md:text-left">Top 10 des produits</h2>
    <Bar id="products-chart-id" class="my-chart" :options="chartOptions" :data="chartDataMostUsedProducts" />
    <h2 class="bg-emerald-800 text-white text-xl md:text-1xl p-1 text-center md:text-left">Nombre de commande / mois (sur un
        an)</h2>
    <Bar id="months-chart-id" class="my-chart" :options="chartOptions" :data="chartDataOrdersByMonth" />
    <h2 class="bg-emerald-800 text-white text-xl md:text-1xl p-1 text-center md:text-left">Nombre de commande / jour (sur un
        mois)</h2>
    <Bar id="days-chart-id" class="my-chart" :options="chartOptions" :data="chartDataOrdersLast30Days" />
</template>

<script>
import axios from 'axios';
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
    components: { Bar },
    data() {
        return {
            chartOptions: {
                responsive: true,
            },
            chartDataMostUsedProducts: {
                labels: [],
                datasets: []
            },
            chartDataOrdersByMonth: {
                labels: [],
                datasets: []
            },
            chartDataOrdersLast30Days: {
                labels: [],
                datasets: []
            },
            mostUsedProducts: null,
            ordersByMonth: null,
            ordersLast30Days: null,
        }
    },
    methods: {
        randomColor() {
            const r = Math.floor(Math.random() * 256);
            const g = Math.floor(Math.random() * 256);
            const b = Math.floor(Math.random() * 256);
            return `rgba(${r}, ${g}, ${b}, 0.5)`;
        },
        fetchStats() {
            const idResto = this.$route.params.id;

            axios.get(`http://localhost:3000/getStatisticRestaurant/${idResto}`)
                .then(response => {
                    console.log(response.data.result);
                    this.mostUsedProducts = response.data.result.mostUsedProducts;
                    this.ordersLast30Days = response.data.result.ordersLast30Days.sort((a, b) => {
                        const aDate = new Date(a.day.split('/').reverse().join('-'));
                        const bDate = new Date(b.day.split('/').reverse().join('-'));
                        return aDate - bDate;
                    });
                    this.ordersByMonth = response.data.result.ordersByMonth.sort((a, b) => {
                        const aDate = new Date(a.month.split('/').reverse().join('-'));
                        const bDate = new Date(b.month.split('/').reverse().join('-'));
                        return aDate - bDate;
                    });

                    // Now that we have the data, update the chart data
                    this.chartDataMostUsedProducts = {
                        labels: this.mostUsedProducts.map(item => item.product),
                        datasets: [{
                            label: 'Most Used Products',
                            data: this.mostUsedProducts.map(item => item.count),
                            backgroundColor: this.mostUsedProducts.map(() => this.randomColor())
                        }]
                    };
                    this.chartDataOrdersByMonth = {
                        labels: this.ordersByMonth.map(item => item.month),
                        datasets: [{
                            label: 'Orders By Month',
                            data: this.ordersByMonth.map(item => item.count),
                            backgroundColor: this.ordersByMonth.map(() => this.randomColor())
                        }]
                    };
                    this.chartDataOrdersLast30Days = {
                        labels: this.ordersLast30Days.map(item => item.day),
                        datasets: [{
                            label: 'Orders Last 30 Days',
                            data: this.ordersLast30Days.map(item => item.count),
                            backgroundColor: this.ordersLast30Days.map(() => this.randomColor())
                        }]
                    };

                })
                .catch(error => {
                    console.error("Erreur lors de la récupération des stats:", error);
                });
        },
    },
    async mounted() {
        try {
            this.fetchStats();
        } catch (error) {
            console.error(error);
        }
    },
};
</script>

<style scoped>
.my-chart {
    max-height: 300px;
    width: 100%;
}
</style>
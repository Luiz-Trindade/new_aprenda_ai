<template>
    <v-container class="fade-in elevation-4">
        <v-row align="center" class="mb-4">
            <v-icon class="mr-2">mdi-chart-bar</v-icon>
            <h1>Estatísticas</h1>
        </v-row>

        <v-row class="d-flex justify-center">
            <v-col class="d-flex" align="center" cols="auto">
                <apexchart type="pie" width="300" :options="chartOptions" :series="series"></apexchart>
            </v-col>
        </v-row>

        <!-- <v-row class="d-flex justify-center">
            <v-col class="d-flex" align="center" cols="auto">
                <apexchart type="pie" width="300" :options="chartOptions" :series="series"></apexchart>
            </v-col>
        </v-row>

        <v-row class="d-flex justify-center">
            <v-col class="d-flex" align="center" cols="auto">
                <apexchart type="pie" width="300" :options="chartOptions" :series="series"></apexchart>
            </v-col>
        </v-row> -->
    </v-container>
</template>

<script>
export default {
    name: "Estatisticas",

    data() {
        return {
            series: [60, 30],
            chartOptions: {
                chart: {
                    width: 380,
                    type: 'pie',
                },
                colors: ['#4CAF50', '#F44336'],
                labels: ['Acertos', 'Erros'],
                responsive: [{
                    breakpoint: 480,
                    options: {
                        chart: {
                            width: 200
                        },
                        legend: {
                            position: 'bottom'
                        }
                    }
                }]
            },
        }
    },

    mounted() {
    },

    methods: {
    }
}
</script>

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

        <!-- Lista de questões -->
        <v-row>
            <v-col >
                <div
                    v-for="(item, index) in quizResults"
                    :key="index"
                    :style="{
                        backgroundColor: item.resultado === 'correto' ? '#4CAF50' : '#F44336',
                        padding: '10px',
                        marginBottom: '10px',
                        color: 'white'
                    }"
                >
                    <div>
                        <strong>Questão {{ index + 1 }}:</strong> {{ item.pergunta }}
                    </div>
                    <br>

                    <div>
                        <strong>Sua resposta:</strong> {{ item.resposta_aluno }}
                    </div>
                    <br>

                    <div>
                        <strong>Explicação:</strong> {{ item.explicacao }}
                    </div>
                </div>
            </v-col>
        </v-row>
    </v-container>

    <AppFooter></AppFooter>
</template>

<script>
export default {
    name: "Estatisticas",
    data() {
        return {
            series: [0, 0],
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
            quizResults: []
        }
    },

    mounted() {
        this.get_correcao();
    },

    methods: {
        get_correcao() {
        const resultadoQuizJSON = localStorage.getItem('resultado_quiz');

        if (resultadoQuizJSON) {
            const resultadoQuiz = JSON.parse(resultadoQuizJSON);

            let corretas = 0;
            let erradas = 0;

            resultadoQuiz.correcao.forEach(item => {
                if (item.resultado === 'correto') {
                    corretas++;
                } else if (item.resultado === 'errado') {
                    erradas++;
                }
            });

            this.series = [corretas, erradas];
            this.quizResults = resultadoQuiz.correcao;
        } else {
            console.warn('Nenhum resultado encontrado no localStorage.');
        }
    }
    }
}
</script>

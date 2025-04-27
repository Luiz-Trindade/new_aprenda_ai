<template>
    <v-container class="fade-in elevation-4">
        <!-- Título de configurações com ícone -->
        <v-row align="center" class="mb-4">
            <v-icon class="mr-2">mdi-cog</v-icon>
            <h1>Configurações</h1>
        </v-row>

        <v-row>
            <v-col>
                <v-btn color="error" @click="resetStatistics">
                    <v-icon class="mr-2">mdi-refresh</v-icon>
                    Resetar Estatísticas
                </v-btn>
            </v-col>

            <v-col>
                <v-btn color="error" @click="resetTopics">
                    <v-icon class="mr-2">mdi-book-open-page-variant</v-icon>
                    Resetar Tópicos
                </v-btn>
            </v-col>
        </v-row>

        <!-- Botão que redireciona para "sobre" -->
        <v-row class="mt-4">
            <v-col>
                <v-btn color="primary" to="/sobre">
                    <v-icon class="mr-2">mdi-information</v-icon>
                    Sobre
                </v-btn>
            </v-col>
        </v-row>
    </v-container>

    <AppFooter></AppFooter>
</template>

<script>
export default {
    data() {
        return {
            age: null,
        }
    },
    methods: {
        setAge() {
            console.log("Idade definida:", this.age);
        },
        resetStatistics() {
            console.log("Estatísticas resetadas");
            localStorage.removeItem('resultado_quiz');
        },
        resetTopics() {
            console.log("Tópicos resetados");
            localStorage.removeItem('topicos');
        }
    }
}
</script>

<style scoped>
</style>

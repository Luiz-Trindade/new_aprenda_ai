<template>
    <v-container class="fade-in elevation-4">
        <!-- Título de configurações com ícone -->
        <v-row align="center" class="mb-4">
            <v-icon class="mr-2">mdi-cog</v-icon>
            <h1>Configurações</h1>
        </v-row>

        <v-form>
            <v-text-field
                v-model="age"
                label="Idade"
                type="number"
            ></v-text-field>

            <v-btn color="primary" @click="setAge">
                <v-icon class="mr-2">mdi-account</v-icon>
                Setar Idade
            </v-btn>
        </v-form>
        <v-divider class="my-4"></v-divider>

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
    </v-container>
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
            // Lógica para setar a idade (ex: salvar em um estado global, API, etc.)
            console.log("Idade definida:", this.age);
        },
        resetStatistics() {
            // Lógica para resetar estatísticas
            console.log("Estatísticas resetadas");
        },
        resetTopics() {
            // Lógica para resetar tópicos
            console.log("Tópicos resetados");
            localStorage.removeItem('topicos');
        }
    }
}
</script>

<style scoped>
</style>
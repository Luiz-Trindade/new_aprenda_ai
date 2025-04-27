<template>
    <v-container>
        <v-card class="pa-4" elevation="6">
            <v-container>
                <v-row dense>
                    <v-col cols="6">
                        <v-btn
                            block
                            color="red"
                            class="elevation-4"
                            height="100"
                            @click="redirect_to('/quiz')"
                        >
                            <span class="text-white font-weight-bold">
                                <v-icon>mdi-play-circle</v-icon>
                                Iniciar
                            </span>
                        </v-btn>
                    </v-col>

                    <v-col cols="6">
                        <v-btn
                            block
                            color="blue"
                            class="elevation-4"
                            height="100"
                        >
                            <span class="text-white font-weight-bold">
                                <v-icon>mdi-chart-bar</v-icon>
                                Estatísticas
                            </span>
                        </v-btn>
                    </v-col>

                    <v-col cols="6">
                        <v-btn
                            block
                            color="green"
                            class="elevation-4"
                            height="100"
                        >
                            <span class="text-white font-weight-bold">
                                <v-icon>mdi-forum</v-icon>
                                Tópicos
                            </span>
                        </v-btn>
                    </v-col>

                    <v-col cols="6">
                        <v-btn
                            block
                            color="purple"
                            class="elevation-4"
                            height="100"
                        >
                            <span class="text-white font-weight-bold">
                                <v-icon>mdi-cog</v-icon>
                                Configurações
                            </span>
                        </v-btn>
                    </v-col>
                </v-row>
            </v-container>
        </v-card>
    </v-container>
</template>
  
<script>
export default {
    name: 'Initial',
    
    data() {
        return {
            // Aqui você pode adicionar dados específicos do componente, se necessário
        };
    },
    
    methods: {
        redirect_to(route) {
            this.$router.push(route);
        }
    }
};
</script>

<style scoped>
</style>

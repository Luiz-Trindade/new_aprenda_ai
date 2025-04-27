<template>
    <v-container class="fade-in" fluid>
        <v-row justify="center">
            <v-col cols="12" md="8">
                <v-card class="pa-5">
                    <v-card-title>
                        Sobre o App
                    </v-card-title>
                    <v-card-text>
                        <p>
                            Este app foi criado como um trabalho de faculdade por Luiz Trindade, Lucas Tavres, José Enrique e Eduardo.
                        </p>
                        <p>
                            Trata-se de um projeto sem fins lucrativos, onde o foco principal é a comunidade e o compartilhamento de conhecimento.
                        </p>
                        <p>
                            A iniciativa demonstra o comprometimento do grupo em contribuir e fortalecer os laços com a comunidade, desenvolvendo soluções que promovam o aprendizado e a colaboração.
                        </p>
                    </v-card-text>
                </v-card>
            </v-col>
        </v-row>
    </v-container>

    <AppFooter></AppFooter>
</template>

<script>
export default {
    name: "Sobre"
}
</script>

<style scoped>
/* Estilos adicionais se necessário */
</style>
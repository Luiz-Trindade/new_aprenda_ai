<template>
    <div class="topicos">
        <h1>Lista de Tópicos</h1>
        <v-row>
            <v-col>
                <v-text-field
                    type="text"
                    v-model="novoTopico"
                    @keyup.enter="adicionarTopico"
                    placeholder="Digite um novo tópico"
                />
                <v-btn color="success" @click="adicionarTopico">
                    <v-icon>mdi-plus-circle</v-icon>
                    Adicionar
                </v-btn>
            </v-col>
        </v-row>
        <v-divider class="my-4"></v-divider>

        <ul>
            <li v-for="(topico, index) in topicos" :key="index">
                {{ topico }}
                <v-btn color="error" @click="removerTopico(index)">
                    <v-icon>mdi-trash-can</v-icon>
                    Remover
                </v-btn>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'Topicos',

    data() {
        return {
            novoTopico: '',
            topicos: [],
        }
    },
    
    created() {
        // Carrega os tópicos armazenados, se existirem
        const topicosSalvos = localStorage.getItem('topicos')
        if (topicosSalvos) {
            this.topicos = JSON.parse(topicosSalvos)
        }
    },
    
    methods: {
        adicionarTopico() {
            if (this.novoTopico.trim()) {
                this.topicos.push(this.novoTopico.trim())
                this.novoTopico = ''
                // Atualiza o localStorage
                localStorage.setItem('topicos', JSON.stringify(this.topicos))
            }
        },
        removerTopico(index) {
            this.topicos.splice(index, 1)
            // Atualiza o localStorage
            localStorage.setItem('topicos', JSON.stringify(this.topicos))
        }
    }
}
</script>

<style scoped>
.topicos {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
}

input {
    padding: 8px;
    width: 70%;
    margin-right: 4px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

button {
    padding: 8px 12px;
    background-color: #007BFF;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}

ul {
    list-style: none;
    padding: 0;
}

li {
    margin-top: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
</style>
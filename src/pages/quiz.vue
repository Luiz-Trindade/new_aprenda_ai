<template>
    <v-container v-show="conteudo_resumo.length > 0" class="fade-in d-flex flex-column">
        <!-- Conteúdo resumido -->
        <v-skeleton-loader v-show="conteudo_resumo.length == 0" type="card" />

        <v-container v-show="!conteudo_revisado && conteudo_resumo.length > 0">
            <p class="mb-2">{{ conteudo_resumo }}</p>
            <v-btn
                class="mb-2 elevation-4 text-left"
                color="green"
                style="width: 100%;"
                variant="outlined"
                @click="confirmar_conteudo_revisado()"
            >
                Ok, entendi!
            </v-btn>
        </v-container>

        <!-- Área de perguntas -->
        <v-container v-show="conteudo_revisado" v-for="(pergunta, index) in lista_de_perguntas" :key="index">
            <QuizOptions
                :pergunta="pergunta.pergunta"
                :alternativas="pergunta.alternativas"
                @answered="recordAnswer"
            />
        </v-container>

        <!-- Botão para corrigir quiz, aparecendo quando já houver respostas -->
        <v-container v-show="conteudo_revisado && responsesString">
            <v-btn color="blue" variant="outlined" @click="corrigirRespostas">
                <v-icon class="mr-2">mdi-check</v-icon>
                Corrigir Quiz
            </v-btn>
        </v-container>

        <!-- Exibição do resultado da correção -->
        <!-- <v-container v-if="resultado_quiz">
            <pre>{{ JSON.stringify(resultado_quiz, null, 2) }}</pre>
        </v-container> -->
    </v-container>

    <AppFooter></AppFooter>
</template>

<script>
export default {
    data() {
        return {
            conteudo_resumo: "",
            responsesString: "",
            resultado_quiz: null,
            conteudo_revisado: false,
            lista_de_perguntas: []
        }
    },

    mounted() {
        this.get_quiz_with_intro();
    },

    methods: {
        confirmar_conteudo_revisado() {
            this.conteudo_revisado = true;
        },

        get_quiz_with_intro() {
            const apiKey = 'AIzaSyDyMncoXz4JNoJV2bRz1Df1YCoxpHRvc30';
            const model = "gemini-2.0-flash";
            const url = `https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent?key=${apiKey}`;
            const storedTopics = JSON.parse(localStorage.getItem('topicos') || '[]');
            const tema = storedTopics.length > 0
                ? storedTopics[Math.floor(Math.random() * storedTopics.length)]
                : "Português para concursos";
            const dificuldade = "Média";

            const prompt = `
                Você é um gerador de conteúdos para quiz.
                Crie um resumo claro e objetivo sobre o tema "${tema}" para 
                revisão de estudos antes do aluno realizar o quiz, citando os 
                principais tópicos referentes ao tema e explicando-os para dar
                um conhecimento prévio ao aluno.

                Em seguida, crie 10 perguntas de dificuldade ${dificuldade} como se fossem perguntas
                de um simulado de faculdade sobre o que foi abordado no resumo do tema
                com 4 alternativas e a resposta correta.

                O resultado deve ser um JSON com as chaves:
                - "resumo": resumo do conteúdo
                - "perguntas": array de perguntas no formato abaixo

                Exemplo de pergunta:
                {
                    "pergunta": "Qual é a capital do Brasil?",
                    "alternativas": ["Brasília", "São Paulo", "Rio de Janeiro", "Salvador"],
                    "resposta": "Brasília"
                }
            `;

            const data = {
                contents: [{
                    parts: [{ text: prompt }]
                }]
            };

            this.$axios.post(url, data, {
                headers: { 'Content-Type': 'application/json' }
            })
            .then(response => {
                let contentText = response.data.candidates[0].content.parts[0].text;
                // Remove blocos markdown
                contentText = contentText.replace(/```(?:json|[a-zA-Z]+)?\n([\s\S]*?)\n```/g, "$1");

                try {
                    const json = JSON.parse(contentText);
                    this.conteudo_resumo = json.resumo;
                    this.lista_de_perguntas = json.perguntas.map(pergunta => ({
                        pergunta: pergunta.pergunta,
                        alternativas: pergunta.alternativas,
                        resposta: pergunta.resposta
                    }));
                } catch (error) {
                    console.error("Erro ao fazer parse do JSON:", error);
                }
            })
            .catch(error => {
                console.error('Erro:', error.response ? error.response.data : error.message);
            });
        },

        recordAnswer({ pergunta, resposta }) {
            // Adiciona uma nova linha à string de respostas
            this.responsesString += `${pergunta}: ${resposta}\n`;
            console.log(this.responsesString);
        },

        corrigirRespostas() {
            const apiKey = 'AIzaSyDyMncoXz4JNoJV2bRz1Df1YCoxpHRvc30';
            const model = "gemini-2.0-flash";
            const url = `https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent?key=${apiKey}`;
            
            // O prompt envia as respostas do usuário e solicita a correção detalhada
            const prompt = `
            Você é um corretor de quizzes. Considere as respostas apresentadas abaixo e corrija cada uma delas comparando com a resposta correta.
            Para cada pergunta, retorne se o aluno acertou ou errou e, em caso de erro, explique o motivo.
            O resultado deve ser um JSON contendo uma chave "correcao" que é um array de objetos com o seguinte formato:
            {
                "pergunta": "<pergunta>",
                "resposta_aluno": "<resposta informada>",
                "resposta_correta": "<resposta correta>",
                "resultado": "correto" ou "errado",
                "explicacao": "<explicação detalhada>"
            }

            Respostas do aluno:
            ${this.responsesString}

            OBS: Use as respostas corretas do quiz conforme gerado anteriormente.
            `;

            const data = {
            contents: [{
                parts: [{ text: prompt }]
            }]
            };

            this.$axios.post(url, data, {
            headers: { 'Content-Type': 'application/json' }
            })
            .then(response => {
            let contentText = response.data.candidates[0].content.parts[0].text;
            // Remove blocos markdown, se houver
            let jsonTextMatch = contentText.match(/```(?:json|[a-zA-Z]+)?\s*([\s\S]*?)\s*```/);
            if (jsonTextMatch && jsonTextMatch[1]) {
                contentText = jsonTextMatch[1].trim();
            } else {
                contentText = contentText.trim();
            }

            try {
                const json = JSON.parse(contentText);
                this.resultado_quiz = json;
                // Armazena o resultado da correção em localStorage
                localStorage.setItem('resultado_quiz', JSON.stringify(json));
                console.log("Resultado da correção:", this.resultado_quiz);
                this.$router.push("/estatisticas");
            } catch (error) {
                console.error("Erro ao fazer parse do JSON de correção:", error);
            }
            })
            .catch(error => {
            console.error('Erro na correção:', error.response ? error.response.data : error.message);
            });
        }
    }
}
</script>

<style scoped>
</style>
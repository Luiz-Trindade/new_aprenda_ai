<template>
    <v-container 
        v-show="conteudo_resumo.length > 0" 
        class="fade-in d-flex flex-column"
    >
        <!--
            Conteúdo resumido para preparar o estudante para o quiz com 
            perguntas relacionadas ao conteúdo    
        -->
        <v-skeleton-loader v-show="conteudo_resumo.length == 0" type="card">
        </v-skeleton-loader>

        <v-container v-show="conteudo_revisado == false && conteudo_resumo.length > 0">
            <p class="mb-2">
                {{conteudo_resumo}}
            </p>

            <v-btn
                class   = "mb-2 elevation-4 text-left"
                color   = "green"
                style   = "width: 100%;"
                variant = "outlined"
                @click  = "confirmar_conteudo_revisado()"
            >
                Ok, entendi!
            </v-btn>
        </v-container>

        <!--
            Área de perguntas
        -->
        <div
            v-show  = "conteudo_revisado == true"
            v-for   = "(pergunta, index) in lista_de_perguntas"
            :key    = "index"
        >
            <QuizOptions
                :pergunta       = "pergunta.pergunta"
                :alternativas   = "pergunta.alternativas"
                @answered       = "recordAnswer"
            />
        </div>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
        return {
            conteudo_resumo     : "",
            responsesString     : "",
            resultAnswers       : "",
            conteudo_revisado   : false,
            lista_de_perguntas  : []
        }
    },

    mounted() {
        this.get_quiz_with_intro();
    },

    methods: {
        confirmar_conteudo_revisado() {
            this.conteudo_revisado = !this.conteudo_revisado;
        },

        get_quiz_with_intro() {
            const apiKey        = 'AIzaSyDyMncoXz4JNoJV2bRz1Df1YCoxpHRvc30';
            const model         = "gemini-2.0-flash";
            const url           = `https://generativelanguage.googleapis.com/v1beta/models/${model}:generateContent?key=${apiKey}`;
            const tema          = "Português para concursos";
            const dificuldade   = "Médio";

            const prompt = `
                Você é um gerador de conteúdos para quiz.
                Crie um resumo claro e objetivo sobre o tema "${tema}" para revisão de estudos.
                Em seguida, crie 10 perguntas com 4 alternativas e a resposta correta.

                O resultado deve ser um JSON com as chaves:
                - "resumo"      : resumo do conteúdo
                - "perguntas"   : array de perguntas no formato abaixo

                Exemplo de pergunta:
                {
                    "pergunta": "Qual é a capital do Brasil?",
                    "alternativas": ["Brasília", "São Paulo", "Rio de Janeiro", "Salvador"]
                }
            `;

            const data = {
                contents: [{
                    parts: [{ text: prompt }]
                }]
            };

            this.$axios.post(url, data, {
                headers: {
                    'Content-Type': 'application/json'
                }
            })
            .then(response => {
                let contentText = response.data.candidates[0].content.parts[0].text;

                // Remove blocos markdown
                contentText = contentText.replace(/```(?:json|[a-zA-Z]+)?\n([\s\S]*?)\n```/g, "$1");

                try {
                    const json = JSON.parse(contentText);
                    this.conteudo_resumo    = json.resumo;
                    this.lista_de_perguntas = json.perguntas.map(pergunta => ({
                        pergunta        : pergunta.pergunta,
                        alternativas    : pergunta.alternativas,
                        resposta        : pergunta.resposta // opcional, pode esconder para não mostrar
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
            // adiciona uma nova linha à string
            this.responsesString += `${pergunta}: ${resposta}\n`;
            console.log(this.responsesString)
        },
    }
  }
  </script>
  
  <style scoped>
  </style>
  
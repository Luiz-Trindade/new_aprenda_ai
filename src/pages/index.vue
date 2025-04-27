<template>
  <v-container class="fade-in d-flex align-center justify-center fill-height">
    <v-card  elevation="6">
      <v-container>
        <v-row>
          <v-col class="text-center">
            <!-- Exibe o título com efeito de digitação -->
            <h1 class="animated-title">{{ displayedText }}</h1>
          </v-col>
        </v-row>

        <v-row >
          <!-- Seus botões existentes -->
          <v-col >
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

          <v-col >
            <v-btn
              block
              color="blue"
              class="elevation-4"
              height="100"
              @click="redirect_to('/estatisticas')"
            >
              <span class="text-white font-weight-bold">
                <v-icon>mdi-chart-bar</v-icon>
                Estatísticas
              </span>
            </v-btn>
          </v-col>

        </v-row>
        
        <v-row >
          <v-col >
            <v-btn
              block
              color="green"
              class="elevation-4"
              height="100"
              @click="redirect_to('/topicos')"
            >
              <span class="text-white font-weight-bold">
                <v-icon>mdi-forum</v-icon>
                Tópicos
              </span>
            </v-btn>
          </v-col>
  
          <v-col >
            <v-btn
              block
              color="purple"
              class="elevation-4"
              height="100"
              @click="redirect_to('/configuracoes')"
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
      // Lista de mensagens a serem exibidas
      messages: [
        "Bem vindo ao Aprenda AI 🎉",
        "Explore nossos conteúdos 📚",
        "Aprenda de forma inovadora 🚀",
        "Aprimore suas habilidades 💡",
        "Desafie-se com quizzes 🧩",
        "Acompanhe suas estatísticas 📊",
        "Interaja com a comunidade 🤝",
        "Acesse tópicos relevantes 🔍"
      ],
      currentMessageIndex: 0,
      displayedText: ''
    };
  },
  mounted() {
    this.startTypingAnimation();
  },
  methods: {
    redirect_to(route) {
      this.$router.push(route);
    },
    startTypingAnimation() {
      const message = this.messages[this.currentMessageIndex];
      let i = 0;
      // Limpa o texto atual antes de iniciar
      this.displayedText = '';

      // Intervalo para "digitar" cada caractere
      const typingInterval = setInterval(() => {
        this.displayedText += message.charAt(i);
        i++;
        if (i === message.length) {
          clearInterval(typingInterval);
          
          setTimeout(() => {
            this.eraseText();
          }, 1000);
        }
      }, 60);
    },
    eraseText() {
      // Intervalo para apagar a mensagem caracter por caracter
      const eraseInterval = setInterval(() => {
        this.displayedText = this.displayedText.slice(0, -1);
        if (this.displayedText.length === 0) {
          clearInterval(eraseInterval);
          // Seleciona a próxima mensagem (em loop)
          this.currentMessageIndex =
            (this.currentMessageIndex + 1) % this.messages.length;
          // Aguarda um instante antes de iniciar nova digitação
          setTimeout(() => {
            this.startTypingAnimation();
          }, 100);
        }
      }, 50);
    }
  }
};
</script>

<style scoped>
.animated-title {
  /* Efeito de cursor piscante */
  display: inline-block;
}
.animated-title::after {
  content: '|';
  animation: blink 1s infinite;
  margin-left: 2px;
}

@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>

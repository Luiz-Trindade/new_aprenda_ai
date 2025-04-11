<template>
    <v-card 
      class="fade-in d-flex flex-column align-center justify-center mb-2"
      style="padding: 10px;"
    >   
        <v-container>
            <p 
            style="text-shadow: 2px 4px 6px rgba(0, 0, 0, 0.7);"
            >
            {{ pergunta }}
            </p>
        </v-container>
  
        <v-container>
            <v-btn
                v-for="(alt, index) in alternativas"
                :key        = "index"
                class       = "mb-2 elevation-4 text-left word-wrap-btn"
                :color      = "colors[index % colors.length]"
                width       = "100%"
                height      = "auto"
                :variant    = "respostaSelecionada === alt ? 'flat' : 'outlined'"
                :disabled   = "respostaSelecionada !== null"
                @click      = "selectAlternative(alt)"
            >
                <div class="button-text">
                    {{ alt }}
                </div>
            </v-btn>
        </v-container>
    </v-card>
</template>
  
<script>
  export default {
    name: 'QuizOptions',
    props: {
      pergunta: {
        type: String,
        required: true
      },
      alternativas: {
        type: Array,
        required: true
      }
    },
    emits: ['answered'],
    data() {
      return {
        colors: ['green', 'blue', 'red', 'yellow'],
        respostaSelecionada: null // aqui guardamos qual foi escolhida
      };
    },
    methods: {
      selectAlternative(alt) {
        if (this.respostaSelecionada !== null) return; // segurança
  
        this.respostaSelecionada = alt;
        this.$emit('answered', {
          pergunta: this.pergunta,
          resposta: alt
        });
      }
    }
  };
</script>
  
<style scoped>
  .word-wrap-btn {
    display: flex;
    align-items: center;
    text-align: left;
    padding: 10px;
    min-height: 60px;
    margin-bottom: 10px;
  }
  .button-text {
    white-space: normal;
    word-wrap: break-word;
    line-height: 1.4;
  }
</style>
  
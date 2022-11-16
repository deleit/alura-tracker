<template>
    <div class="is-flex is-align-items-center is-justify-content-space-between">
        <Cronometro :tempoEmSegundos="tempoEmSegundos" />
        <Botao 
            :disabled="cronometroRodando || idProjeto === ''"
            label="play"
            icon="fas fa-play"
            @clicado="iniciar"
        />
        <Botao 
            :disabled="!cronometroRodando"
            label="stop"
            icon="fas fa-stop"
            @clicado="finalizar"
        />
    </div>
</template>

<script lang="ts">
    import { defineComponent } from 'vue';
    import Cronometro from './Cronometro.vue';
    import Botao from './Botao.vue';

    export default defineComponent({
        name: 'Temporizador',
        emits: ['aoTemporizadorFinalizado'],
        components: {
            Cronometro,
            Botao
        },
        props: {
            idProjeto: {
                type: String
            }
        },
        data () {
            return {
                tempoEmSegundos: 0,
                cronometro: 0,
                cronometroRodando: false
            }
        },
        methods: {
            iniciar () {
                this.cronometro = setInterval(() => {
                    this.tempoEmSegundos++;
                }, 1000);
                this.cronometroRodando = true;
            },
            finalizar () {
                this.cronometroRodando = false;
                this.$emit('aoTemporizadorFinalizado', this.tempoEmSegundos);
                clearInterval(this.cronometro);
                this.tempoEmSegundos = 0;
            }
        }
    });
</script>
<template>
    <Box>
        <div class="columns container">
            <div class="column is-4">
                {{ tarefa.descricao || 'Tarefa sem descrição' }}
            </div>
            <div class="column is-3">
                {{ tarefa.projeto?.nome || 'N/D' }}
            </div>
            <div class="column">
                <Cronometro :tempoEmSegundos="tarefa.duracaoEmSegundos" />
            </div>
            <div class="column">
                <div class="button is-info" @click="tarefaClicada">
                    <span class="icon is-small">
                        <i class="fas fa-pencil-alt"></i>
                    </span>
                </div>
            </div>
        </div>
    </Box>
</template>

<script lang="ts">
    import ITarefa from '@/interfaces/ITarefa';
    import { defineComponent, PropType } from 'vue';
    import Cronometro from './Cronometro.vue';
    import Box from './Box.vue';

    export default defineComponent({
        name: 'Tarefa',
        emits: ['aoTarefaClicada'],
        components: {
            Cronometro,
            Box
        },
        props: {
            tarefa: {
                type: Object as PropType<ITarefa>,
                required: true
            }
        },
        methods: {
            tarefaClicada () : void {
                this.$emit('aoTarefaClicada', this.tarefa)
            }
        },
    });
</script>
<style scoped>
.container {
    align-items: center;
}
</style>
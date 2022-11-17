<template>
    <div class="box formulario">
        <div class="columns">
            <div class="column is-5" role="form" aria-label="Formulário para criação de uma nova tarefa">
                <input 
                    type="text" 
                    class="input" 
                    placeholder="Qual tarefa você deseja iniciar?"
                    v-model="descricao"
                >
            </div>

            <div class="column is-3">
                <div class="select">
                    <select v-model="idProjeto">
                        <option value="" selected disabled>Selecione o projeto</option>
                        <option 
                            v-for="projeto in projetos"
                            :value="projeto.id"
                            :key="projeto.id"    
                        >
                            {{ projeto.nome }}
                        </option>
                    </select>
                </div>
            </div>

            <div class="column">
                <Temporizador 
                    @aoTemporizadorFinalizado="finalizarTarefa"
                />
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { TipoNotificacao } from '@/interfaces/INotificacao';
import { key } from '@/store';
import { NOTIFICAR } from '@/store/tipo-mutacoes';
import { computed, ref } from '@vue/reactivity';
import { defineComponent } from 'vue';
import { useStore } from 'vuex';
import Temporizador from './Temporizador.vue';

export default defineComponent({
    name: 'Formulario',
    emits: ['aoSalvarTarefa'],
    components: {
        Temporizador
    },
    setup (props, { emit }) {
        const store = useStore(key);
        const descricao = ref('');
        const idProjeto = ref('');
        const projetos = computed(() => store.state.projeto.projetos);

        const finalizarTarefa = (tempoDecorrido: number) : void => {
            const projeto = projetos.value.find((p) => p.id == idProjeto.value);
            if (projeto) {
                emit('aoSalvarTarefa', {
                    duracaoEmSegundos: tempoDecorrido,
                    descricao: descricao.value,
                    projeto: projetos.value.find(proj => proj.id == idProjeto.value)
                });
                descricao.value = '';
            } else {
                store.commit(NOTIFICAR, {
                    titulo: 'Ops!',
                    texto: 'Selecione um projeto antes de finalizar a tarefa',
                    tipo: TipoNotificacao.FALHA,
                });
            }
        }

        return {
            descricao,
            idProjeto,
            finalizarTarefa,
            projetos
        }
    }
})
</script>

<style>
.formulario {
    background-color: var(--bg-primario);
    color: var(--texto-primario);
}
</style>
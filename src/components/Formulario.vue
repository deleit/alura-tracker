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
                        <option value="">Selecione o projeto</option>
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
import { computed } from '@vue/reactivity';
import { defineComponent } from 'vue';
import { useStore } from 'vuex';
import Temporizador from './Temporizador.vue';

export default defineComponent({
    name: 'Formulario',
    emits: ['aoSalvarTarefa'],
    components: {
        Temporizador
    },
    data () {
        return {
            descricao: '',
            idProjeto: '',
        }
    },
    methods: {
        finalizarTarefa (tempoDecorrido: number) : void {
            const projeto = this.projetos.find((p) => p.id == this.idProjeto);
            if (projeto) {
                this.$emit('aoSalvarTarefa', {
                    duracaoEmSegundos: tempoDecorrido,
                    descricao: this.descricao,
                    projeto: this.projetos.find(proj => proj.id == this.idProjeto)
                });
                this.descricao = '';
            } else {
                this.store.commit(NOTIFICAR, {
                    titulo: 'Ops!',
                    texto: 'Selecione um projeto antes de finalizar a tarefa',
                    tipo: TipoNotificacao.FALHA,
                });
            }
        }
    },
    setup () {
        const store = useStore(key);
        return {
            projetos: computed(() => store.state.projetos),
            store
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
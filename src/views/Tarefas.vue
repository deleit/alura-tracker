<template>
    <Formulario @aoSalvarTarefa="salvarTarefa" />
    <div class="lista">
        <Tarefa 
            v-for="(tarefa, index) in tarefas" 
            :key="index" 
            :tarefa="tarefa" 
            @aoTarefaClicada="selecionarTarefa"
        />
        <Box v-if="listaEstaVazia">
            Você não está muito produtivo hoje :(
        </Box>
        <Modal v-if="tarefaSelecionada !== null" :mostrar="mostrarModal">
            <template v-slot:cabecalho>
                <p class="modal-card-title">Editar tarefa</p>
                <button @click="fecharModal" class="delete" aria-label="close"></button>
            </template>
            <template v-slot:corpo>
                <div class="field">
                    <label for="descricaoDaTarefa" class="label">
                        Descrição
                    </label>
                    <input type="text" class="input" v-model="tarefaSelecionada.descricao" id="descricaoDaTarefa">
                </div>
            </template>
            <template v-slot:rodape>
                <button @click="alterarTarefa" class="button is-success">Salvar alterações</button>
                <button @click="fecharModal" class="button">Cancelar</button>
            </template>
        </Modal>
    </div>
</template>
  
<script lang="ts">
import { computed, defineComponent } from 'vue';
import Formulario from '../components/Formulario.vue';
import Tarefa from '../components/Tarefa.vue';
import Box from '../components/Box.vue';
import { useStore } from '@/store';
import { ALTERAR_TAREFA, CADASTRAR_TAREFA, OBTER_PROJETOS, OBTER_TAREFAS } from '@/store/tipo-acoes';
import useNotificador from '@/hooks/notificador';
import ITarefa from '@/interfaces/ITarefa';
import Modal from '@/components/Modal.vue';

export default defineComponent({
    name: 'Tarefas',
    components: {
        Formulario,
        Tarefa,
        Box,
        Modal
    },
    data () {
        return {
            tarefaSelecionada: null as ITarefa | null
        }
    },
    methods: {
        salvarTarefa (tarefa: ITarefa) : void {
            this.store.dispatch(CADASTRAR_TAREFA, tarefa)
        },
        selecionarTarefa (tarefa: ITarefa) : void {
            this.tarefaSelecionada = tarefa;
        },
        fecharModal () : void {
            this.tarefaSelecionada = null;
        },
        alterarTarefa () : void {
            this.store.dispatch(ALTERAR_TAREFA, this.tarefaSelecionada)
                .then(() => this.fecharModal())
        }
    },
    computed: {
        listaEstaVazia(): boolean {
            return this.tarefas?.length === 0
        },
        mostrarModal(): boolean {
            return this.tarefaSelecionada !== null;
        }
    },
    setup () {
        const store = useStore();
        store.dispatch(OBTER_TAREFAS);
        store.dispatch(OBTER_PROJETOS);
        const { notificar } = useNotificador();
        return {
            tarefas: computed(() => store.state.tarefa.tarefas),
            store,
            notificar
        }
    }
});
</script>
  
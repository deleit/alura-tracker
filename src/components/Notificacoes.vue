<template>
    <div class="notificacoes">
        <article 
            v-for="notificacao in notificacoes"
            class="message" 
            :class="contexto[notificacao.tipo]" 
            :key="notificacao.id"
        >
            <div class="message-header">
                {{ notificacao.titulo }}
            </div>
            <div class="message-body">
                {{ notificacao.texto }}
            </div>
        </article>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { useStore } from '@/store';
import { computed } from '@vue/reactivity';
import { TipoNotificacao } from '@/interfaces/INotificacao';

export default defineComponent({
    name: 'Notificacoes',
    data () {
        return {
            contexto: {
                [TipoNotificacao.SUCESSO]: 'is-success',
                [TipoNotificacao.ATENCAO]: 'is-warning',
                [TipoNotificacao.FALHA]: 'is-danger'
            }
        }
    },
    setup () {
        const store = useStore();
        return {
            notificacoes: computed(() => store.state.notificacoes)
        }
    }
})

</script>

<style scoped>
.notificacoes {
    position: absolute;
    right: 0;
    bottom: 0;
    width: 300px;
    padding: 8px;
    z-index: 99;
}
</style>
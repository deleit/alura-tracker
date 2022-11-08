<template>
    <button class="button" :disabled="disabled" @click="clicado">
        <span class="icon">
            <i :class="icon"></i>
        </span>
        <span>{{ label }}</span>
    </button>
</template>

<script lang="ts">
    import { defineComponent } from 'vue';

    export default defineComponent({
        name: 'Botao',
        emits: ['clicado'],
        props: {
            disabled: {
                type: Boolean
            },
            label: {
                type: String,
                required: true
            },
            icon: {
                type: String,
                required: true
            },
        },
        methods: {
            clicado () : void {
                this.$emit('clicado');
            }
        }
    });
</script>
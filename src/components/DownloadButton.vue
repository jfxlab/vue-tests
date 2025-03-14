<script setup lang="ts">
import { defineProps, computed } from 'vue';
import Button from 'primevue/button';

// Define a propriedade com valor padrão para evitar erro de undefined
const props = defineProps<{
  jsonString?: string; // Permite que seja undefined inicialmente
}>();

// Computa se o botão deve estar ativo (só ativa se houver conteúdo JSON válido)
const isDisabled = computed(() => !props.jsonString || !props.jsonString.trim());

const downloadFile = () => {
  if (isDisabled.value) return;

  const blob = new Blob([props.jsonString], { type: 'application/json' });
  const link = document.createElement('a');
  link.href = URL.createObjectURL(blob);
  link.download = 'secrets.tfvars.json';
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};
</script>

<template>
  <Button :disabled="isDisabled" @click="downloadFile">Baixar secrets.tfvars.json</Button>
</template>

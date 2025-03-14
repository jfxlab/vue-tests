<template>
  <div>
    <h1>Gerenciador de Parâmetros</h1>

    <!-- Apenas ouvimos o evento -->
    <KVForm @update:parameters="handleParametersUpdate" @update:encodedString="encodedString  = $event" />

    <h3>Parâmetros Atuais:</h3>


    <pre>{{ encodedString }}</pre>

    <DownloadButton :jsonString="encodedString" />

  </div>
</template>

<script setup lang="ts">
import KVForm from '@/components/KVForm.vue';
import { ref } from 'vue';
import DownloadButton from '@/components/DownloadButton.vue';
interface Parameter {
  key: string;
  value: string;
}

const parameters = ref<Parameter[]>([]);
const encodedString = ref<string>();

const handleParametersUpdate = (updatedParams: Parameter[]) => {
  parameters.value = updatedParams;
};
</script>

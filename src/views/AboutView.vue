<template>
  <div class="card">
    <h2 class="mb-2">Adicionar Parâmetros</h2>

    <div v-for="(item, index) in parameters" :key="index" class="field-row mb-5">
      <div class="input-group">
        <IftaLabel>
          <InputText v-model="item.key" :id="'key-' + index"/>
          <label for="key">Chave</label>
        </IftaLabel>
      </div>
      <div class="input-group">
        <IftaLabel>
          <InputText v-model="item.value" :id="'value-' + index"/>
          <label for="value">Valor</label>
        </IftaLabel>
      </div>
      <div class="remove-button">
        <Button 
          v-if="index > 0"
          icon="pi pi-times" 
          class="p-button-danger p-button-sm" 
          severity="danger"
          variant="text"
          @click="removeField(index)" 
        />
      </div>
    </div>

    <Button label="Adicionar +" icon="pi pi-plus" class="p-button-text" @click="addField" />

    <Divider />

    <Button label="Enviar" class="p-button-success" @click="submitForm" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import InputText from 'primevue/inputtext';
import Button from 'primevue/button';
import Divider from 'primevue/divider';
import IftaLabel from 'primevue/iftalabel';

interface Parameter {
  key: string;
  value: string;
}

const parameters = ref<Parameter[]>([{ key: '', value: '' }]);

const addField = () => {
  parameters.value.push({ key: '', value: '' });
};

const removeField = (index: number) => {
  parameters.value.splice(index, 1);
};

const submitForm = () => {
  const filteredParameters = parameters.value.filter(param => param.key.trim() !== '' && param.value.trim() !== '');
  console.log('Enviando:', filteredParameters);
};
</script>

<style scoped>
.card {
  max-width: 500px;
  margin: auto;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 5px;
}

.field-row {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

.input-group {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.remove-button {
  align-self: flex-end; 
  margin-bottom: 0.15rem;
}

</style>

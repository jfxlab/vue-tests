<template>
    <div>
      <Toast />
      <FileUpload mode="basic"  :auto="true"  customUpload @uploader="handleFileUpload" chooseLabel="Selecionar Arquivo" />
      
      <Card v-if="base64Content" class="mt-4">
        <template #title> Base64 do Arquivo </template>
        <template #content>
          <textarea v-model="base64Content" class="w-full p-2 border rounded" rows="6" readonly></textarea>
        </template>
      </Card>
      {{ base64Content }}
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from "vue";
  import { useToast } from "primevue/usetoast";
  import FileUpload from "primevue/fileupload";
  import Card from "primevue/card";
  import Toast from "primevue/toast";
  
  const base64Content = ref<string | null>(null);
  const toast = useToast();
  
  const allowedExtensions = [".crt", ".txt", ".yaml", ".json"];

  const handleFileUpload = (event: { files: File[] }) => {
    const file = event.files[0];
    
    if (!file) return;
    
    const fileExtension = file.name.slice(file.name.lastIndexOf(".")).toLowerCase();
    if (!allowedExtensions.includes(fileExtension)) {
        toast.add({ severity: "error", summary: "Erro", detail: "Tipo de arquivo não permitido.", life: 3000 });
        return;
    }

    if (file.size > 32 * 1024) {
      toast.add({ severity: "error", summary: "Erro", detail: "O arquivo deve ter no máximo 32KB.", life: 3000 });
      return;
    }
    
    const reader = new FileReader();
    reader.onload = () => {
      toast.add({severity: "info", summary: "Uploaded", detail: "O arquivo foi enviado com sucesso!"})
      base64Content.value = reader.result as string;
    };
    reader.readAsDataURL(file);
  };
  </script>
  
  <style scoped>
  textarea {
    font-family: monospace;
  }
  </style>
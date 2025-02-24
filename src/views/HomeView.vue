<script setup>
import { ref } from 'vue';

const jobDescription = ref('');
const selectedFile = ref(null);
const loading = ref(false);
const error = ref(null);
const success = ref(false);
const evaluacion = ref('');

const handleFileChange = (event) => {
  selectedFile.value = event.target.files[0];
};

const submitCV = async () => {
  try {
    loading.value = true;
    error.value = null;
    
    const formData = new FormData();
    formData.append('file', selectedFile.value);
    formData.append('job_description', jobDescription.value);

    const response = await fetch('http://localhost:8000/procesar-cv/', {
      method: 'POST',
      body: formData
    });

    if (!response.ok) {
      throw new Error('Error al procesar la solicitud');
    }

    const data = await response.json();
    success.value = true;
    evaluacion.value = data.evaluacion;
    console.log('Respuesta:', data);
  } catch (err) {
    error.value = err.message;
    console.error('Error:', err);
  } finally {
    loading.value = false;
  }
};

const formatMarkdown = (text) => {
  if (!text) return '';
  return text
    .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
    .replace(/\n\n/g, '</p><p>')
    .replace(/\n/g, '<br>');
};
</script>

<template>
  <div class="job-application">
    <div class="job-description">
      <h2>Descripción del puesto</h2>
      <textarea
        v-model="jobDescription"
        placeholder="Ingrese la descripción del puesto aquí..."
        rows="6"
      ></textarea>
    </div>
    <div class="cv-upload">
      <h2>Subir CV</h2>
      <label for="file-upload" class="file-upload-label">
        <span class="material-icons">upload_file</span>
        Seleccionar archivo
      </label>
      <input
        id="file-upload"
        type="file"
        @change="handleFileChange"
        accept=".pdf,.doc,.docx"
      />
      <p v-if="selectedFile">{{ selectedFile.name }}</p>
      <button 
        @click="submitCV" 
        :disabled="loading || !selectedFile || !jobDescription"
        class="file-upload-label"
        style="border: none; margin-top: 1rem; display: block;"
      >
        {{ loading ? 'Enviando...' : 'Enviar CV' }}
      </button>
      <p v-if="error" style="color: #d32f2f;">{{ error }}</p>
      <p v-if="success" style="color: #2e7d32;">¡CV enviado con éxito!</p>
    </div>
    
    <!-- Sección de Resultados -->
    <div v-if="evaluacion" class="evaluation-results">
      <h2>Resultados de la Evaluación</h2>
      <div 
        class="evaluation-content"
        v-html="formatMarkdown(evaluacion)"
      ></div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap');
@import url('https://fonts.googleapis.com/icon?family=Material+Icons');

.job-application {
  font-family: 'Roboto', sans-serif;
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
  max-width: 1200px;
  margin: 2rem auto;
  padding: 2rem;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.job-description, .cv-upload {
  flex: 1;
  min-width: 300px;
}

.evaluation-results {
  flex-basis: 100%;
  margin-top: 2rem;
  padding: 1.5rem;
  background-color: #f5f5f5;
  border-radius: 8px;
}

.evaluation-content {
  font-size: 1rem;
  line-height: 1.6;
}

.evaluation-content p {
  margin: 1rem 0;
}

.evaluation-content strong {
  color: #1976d2;
}

h2 {
  color: #1976d2;
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

textarea {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  font-family: 'Roboto', sans-serif;
  font-size: 1rem;
  resize: vertical;
}

.file-upload-label {
  display: inline-flex;
  align-items: center;
  padding: 0.5rem 1rem;
  background-color: #1976d2;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.file-upload-label:hover {
  background-color: #1565c0;
}

.file-upload-label .material-icons {
  margin-right: 0.5rem;
}

input[type="file"] {
  display: none;
}

p {
  margin-top: 0.5rem;
  color: #616161;
}
</style>
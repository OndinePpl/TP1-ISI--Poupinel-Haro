<script setup>
import { ref } from 'vue'

const tracks = ref([
  { title: 'Exemple de musique en ligne', url: 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3', type: 'link' }
]);

const inputTitle = ref('');
const inputUrl = ref('');
const selectedFile = ref(null);
const addMode = ref('link');

function handleFileChange(event) {
  selectedFile.value = event.target.files[0];
  if (selectedFile.value && !inputTitle.value) {
    inputTitle.value = selectedFile.value.name;
  }
}

function addTrack() {
  if (addMode.value === 'link' && inputUrl.value) {
    tracks.value.push({
      title: inputTitle.value || 'Sans titre',
      url: inputUrl.value,
      type: 'link'
    });
    inputUrl.value = '';
  } else if (addMode.value === 'file' && selectedFile.value) {
    const localUrl = URL.createObjectURL(selectedFile.value);
    tracks.value.push({
      title: inputTitle.value || selectedFile.value.name,
      url: localUrl,
      type: 'file'
    });
    selectedFile.value = null;
  }
  inputTitle.value = '';
}
</script>

<template>
  <div id="app">
    <h1>🎵 Ma Boîte Musicale</h1>
    <section class="add-form">
      <h2>Ajouter un titre</h2>
      <div>
        <label>
          <input type="radio" v-model="addMode" value="link" /> Lien URL
        </label>
        <label>
          <input type="radio" v-model="addMode" value="file" /> Fichier local
        </label>
      </div>
      <div>
        <input type="text" v-model="inputTitle" placeholder="Nom du titre (laisser vide pour auto)" />
      </div>
      <div v-if="addMode === 'link'">
        <input type="url" v-model="inputUrl" placeholder="https://exemple.com/audio.mp3" />
      </div>
      <div v-else>
        <input type="file" accept="audio/*" @change="handleFileChange" />
      </div>

      <button @click="addTrack">Ajouter à la playlist</button>
    </section>
    <section class="playlist">
      <h2>Liste de lecture</h2>
      <ul>
        <li v-for="(track, index) in tracks" :key="index">
          <strong>{{ track.title }}</strong> ({{ track.type }})
        </li>
      </ul>
    </section>
  </div>
</template>

<style>
#app {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
}
section {
  margin-bottom: 20px;
  padding: 15px;
  border: 1px solid #ccc;
  border-radius: 8px;
}
</style>
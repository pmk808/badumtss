<template>
    <div class="drum-pad">
        <button v-for="sound in sounds" :key="sound.name" @click="playSound(sound.id)">
            {{ sound.name }}
        </button>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const FREESOUND_API_KEY = process.env.VUE_APP_FREESOUND_API_KEY;
console.log('API Key:', FREESOUND_API_KEY);

const sounds = ref([
    { name: 'Kick', id: '132584' },
    { name: 'Snare', id: '132583' },
    { name: 'Hi-Hat', id: '132582' },
    // Add more sounds as needed
]);

const playSound = async (soundId) => {
    try {
        const response = await axios.get(`https://freesound.org/apiv2/sounds/${soundId}/?token=${FREESOUND_API_KEY}`);
        const previewUrl = response.data.previews['preview-hq-mp3'];
        
        const audio = new Audio(previewUrl);
        audio.play();
    } catch (error) {
        console.error('Error playing sound:', error);
    }
};
</script>

<style scoped>
.drum-pad {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
}

button {
    padding: 20px;
    font-size: 18px;
    cursor: pointer;
}
</style>
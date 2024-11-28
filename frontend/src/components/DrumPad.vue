<template>
    <div class="drum-pad">
        <!-- Render buttons dynamically for each sound -->
        <button 
            v-for="sound in sounds" 
            :key="sound.name" 
            @click="playSound(sound.id)">
            {{ sound.name }}
        </button>
    </div>
</template>

<script setup>
// Import Vue features and Axios
import { ref } from 'vue';
import axios from 'axios';

// Use environment variables to hide sensitive information
const FREESOUND_API_KEY = process.env.VUE_APP_FREESOUND_API_KEY; // Masked API key
const FREESOUND_BASE_URL = process.env.VUE_APP_FREESOUND_BASE_URL || 'https://freesound.org/apiv2'; // Configurable base URL

// Log the API key in development only (DO NOT LOG in production)
if (process.env.NODE_ENV === 'development') {
    console.log('API Key:', FREESOUND_API_KEY);
}

// Define sound buttons with placeholder IDs (replace with real IDs as needed)
const sounds = ref([
    { name: 'Kick', id: 'PLACEHOLDER_ID_1' },
    { name: 'Snare', id: 'PLACEHOLDER_ID_2' },
    { name: 'Hi-Hat', id: 'PLACEHOLDER_ID_3' },
    // Add more sounds here
]);

// Function to play the sound using the Freesound API
const playSound = async (soundId) => {
    try {
        // Construct the API URL
        const response = await axios.get(`${FREESOUND_BASE_URL}/sounds/${soundId}/?token=${FREESOUND_API_KEY}`);
        
        // Access the sound preview URL
        const previewUrl = response.data.previews['preview-hq-mp3'];

        // Create an audio object and play the sound
        const audio = new Audio(previewUrl);
        audio.play();
    } catch (error) {
        // Log only non-sensitive errors in production
        if (process.env.NODE_ENV === 'development') {
            console.error('Error playing sound:', error);
        } else {
            console.error('An error occurred while playing the sound.');
        }
    }
};
</script>

<style scoped>
/* Scoped styling for the drum pad buttons */
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

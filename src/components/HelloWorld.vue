<script setup>
import { Configuration, OpenAIApi } from 'openai';
import { ref } from 'vue';
const image_url = ref(false);
const image_size = ref("1024x1024");
const sizeInput = ref("1024x1024");
const loading = ref(false);
const prompt = ref("");

defineProps({
  msg: String
});

const OPENAI_API_KEY = "YOUR_OPEN_AI_HERE"

/* If you create .env with OPENAI_API_KEY=
replace import.meta.env to process.env 
  if you are not using VITE*/

const configuration = new Configuration({
  apiKey: OPENAI_API_KEY,
});

const openai = new OpenAIApi(configuration);

const generate = async () => {
  if(prompt.value != ""){
    loading.value = true;
    const response = await openai.createImage({
      prompt: `${prompt.value}`,
      n: 1,
      size: `${image_size.value}`
    })
    image_url.value = response.data.data[0].url
    loading.value = false;
    prompt.vaue = "";
  }
}
const updateSize = () => {
  image_size.value = sizeInput.value;
}
</script>

<template>
  <h1>AI Image Generator 👨‍💻</h1>
  <form @submit.prevent="generate">
    <div class="flex">
      <input type="text" v-model="prompt" required placeholder="What is in your mind ? 🧠">
      <select v-model="sizeInput" @change="updateSize">
        <option value="1024x1024" selected>1024x1024</option>
        <option value="512x512">512x512</option>
        <option value="256x256">256x256</option>
      </select>
      <button type="submit">🚀</button>
    </div>
  </form>
  <div class="loader" v-if="loading"></div>
  <img v-if="image_url" :src="image_url">
</template>

<style scoped>
.read-the-docs {
  color: #888;
}

form {
  max-width: 550px;
  margin: auto;
  padding: 20px 1em;
  width: 100%;
}

.flex {
  display: flex;
}

input {
  padding: 12px;
  border-radius: 5px;
  outline: none;
  border: none;
  width: 100%;
}

select {
  padding: 12px;
  border-radius: 5px;
  outline: none;
  border: none;
  margin-left: 10px;
}

button {
  background-color: #ec1947;
  margin-left: 10px;
  border-radius: 5px;
  display: flex;
}

input::placeholder {
  color: rgb(204, 204, 204);
}

.loader {
  border: 8px solid #f3f3f3;
  border-radius: 50%;
  border-top: 8px solid #ec1947;
  border-bottom: 8px solid #ec1947;
  width: 60px;
  height: 60px;
  -webkit-animation: spin 2s linear infinite;
  /* Safari */
  animation: spin 2s linear infinite;
  margin: auto;
}

/* Safari */
@-webkit-keyframes spin {
  0% {
    -webkit-transform: rotate(0deg);
  }

  100% {
    -webkit-transform: rotate(360deg);
  }
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
</style>

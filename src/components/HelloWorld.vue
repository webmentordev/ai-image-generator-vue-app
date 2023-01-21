<script setup>
import { Configuration, OpenAIApi } from 'openai';
import { ref } from 'vue';
const image_url = ref(false);
const image_size = ref("512x512");
const sizeInput = ref("512x512");
const loading = ref(false);
const prompt = ref("");

const OPENAI_API_KEY = "OPEN_KEY_HERE"

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
        <option value="512x512" selected>512x512</option>
        <option value="256x256">256x256</option>
        <option value="1024x1024">1024x1024</option>
        
      </select>
      <button type="submit">🚀</button>
    </div>
  </form>
  <div v-if="loading" class="loading-div">
    <div class="loader"></div>
    <p>Generating your masterpiece!</p>
  </div>
  <div class="img-box" v-if="image_url">
    <a class="donwload-img" :href="image_url" download>Download</a>
    <img :src="image_url">
  </div>
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

.loading-div{
  text-align: center;
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

img{
  border-radius: 10px;
}
.img-box{
  position: relative;
}
.donwload-img{
  padding: 10px 20px;
  position: absolute;
  top: 15px;
  left: 45%;
  z-index: 5;
  color: white;
  font-weight: 500;
  border-radius: 10px;
  background-color: #ec1947;
}
</style>

<template>
  <h1>Omoikane</h1>
  <input type="text" v-model="inputText" />
  <button type="button" @click="fetchResponse">Ask omoikane</button>
  <button type="button" @click="speech('salut')">speak</button>

  {{ response }}
</template>

<script setup>
import { ref } from "vue";
import { Configuration, OpenAIApi } from "openai";



const configuration = new Configuration({
  organization: "org-sLV703vpHvgJpm8oikvH1y4m",
  apiKey: process.env.VUE_APP_KEY,
});

const openai = new OpenAIApi(configuration);



let inputText = ref("");
let response = ref("")
// let tts

async function fetchResponse() {
  const gptResponse = await openai.createCompletion(
    {
      "model": "text-davinci-003",
  "prompt": `${inputText.value}`,
  "max_tokens": 250,
  "temperature": 0.9,
  "top_p": 1,
  "n": 1,
    },

  );
  response.value = gptResponse.data.choices[0].text
  const utterance = new SpeechSynthesisUtterance("Salut je vais bien")
  utterance.volume = 10
  utterance.lang = "fr-FR"
  console.log(speechSynthesis.getVoices())
  console.log(utterance)
  speechSynthesis.cancel()
  speechSynthesis.speak(utterance)

}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

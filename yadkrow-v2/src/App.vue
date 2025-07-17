<template>
  <div class="container">
    <form @submit.prevent="submitPost" class="form">
      <h1 class="title">yadkroW v2.0</h1>

      <textarea v-model="postContent" placeholder="Type what you would like to share on LinkedIn, here!" required
        class="input"></textarea>

      <button type="submit" class="button">
        Share on LinkedIn
      </button>

      <p style="font-size: 13px;">Built with dedication by me (Jose Jimenez Artavia) for my final interview at Workday</p>
      <p style="font-size: 12px;">Powered by Microsoft Azure Serverless APIs</p>

      <p v-if="responseMessage" class="success">{{ responseMessage }}</p>
      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const postContent = ref('')
const responseMessage = ref('')
const errorMessage = ref('')

const submitPost = async () => {
  responseMessage.value = ''
  errorMessage.value = ''

  try {
    const response = await axios.post('https://yadkrow-api.azure-api.net/yadkrow/v2/postToLinkedIn', {
      text: postContent.value
    })

    if (response.status === 201 || response.status === 200) {
      responseMessage.value = '✅ Dont worry you got this!'
      postContent.value = ''
    } else {
      errorMessage.value = '❌ Ups! Your post could not complete its journey.'
    }
  } catch (error) {
    errorMessage.value = '⚠️ Oh No! This thing is not working, please contact your IT Admin.'
    console.error(error)
  }
}
</script>

<style>
body {
  margin: 0;
  background-color: #0875E1;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  color: white;
}

.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  background-color: #0875E1;
}

.form {
  width: 100%;
  max-width: 500px;
  background-color: #0875E1;
  border: 2px solid white;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 0 10px white;
}

.title {
  font-size: 28px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 20px;
}

.input {
  width: 100%;
  height: 120px;
  border-radius: 8px;
  padding: 10px;
  font-size: 16px;
  margin-bottom: 20px;
  color: black;
  background-color: white;
  box-sizing: border-box;
  border: none;
}

.input::placeholder {
  color: black;
  opacity: 0.7;
}

.button {
  width: 100%;
  background-color: #F29C1F;
  color: white;
  padding: 14px;
  font-size: 16px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
}

.button:hover {
  background-color: #e08a12;
}

.success {
  color: #b2f2bb;
  margin-top: 16px;
  text-align: center;
}

.error {
  color: #ffc9c9;
  margin-top: 16px;
  text-align: center;
}
</style>
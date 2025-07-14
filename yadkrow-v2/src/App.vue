<template>
  <div class="container">
    <form @submit.prevent="submitPost" class="form">
      <h1 class="title">Yadkrow v2.0</h1>
      <h3 class="subtitle">Powered by Azure API Management</h3>

      <textarea v-model="postContent" placeholder="Type what you would like to share on LinkedIn, here!" required
        class="input"></textarea>

      <button type="submit" class="button">
        Share on LinkedIn
      </button>

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
    const response = await axios.post('<YOUR_APIM_ENDPOINT_URL>', {
      content: postContent.value
    })

    if (response.status === 201) {
      responseMessage.value = '✅ Post published successfully!'
      postContent.value = ''
    } else {
      errorMessage.value = '❌ Something went wrong while publishing.'
    }
  } catch (error) {
    errorMessage.value = '⚠️ Unexpected error occurred.'
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
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
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
  border: none;
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

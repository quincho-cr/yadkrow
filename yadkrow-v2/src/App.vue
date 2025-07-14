<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100">
    <form 
      @submit.prevent="submitPost" 
      class="bg-white shadow-xl rounded-lg px-8 pt-6 pb-8 w-full max-w-lg"
    >
      <div class="mb-4">
        <img 
          src="https://www.workday.com/content/dam/web/en-us/images/global/workday-logo.svg" 
          alt="Workday" 
          class="mx-auto mb-4 w-32"
        />
        <h2 class="text-2xl text-center font-bold text-[#0875e1]">LinkedIn Post Creator</h2>
      </div>
      <textarea 
        v-model="postContent"
        placeholder="Compose your LinkedIn post..."
        required
        class="w-full border border-gray-300 rounded-lg p-3 focus:border-[#0875e1] focus:ring-1 focus:ring-[#0875e1] outline-none"
      ></textarea>
      <button
        type="submit"
        class="w-full mt-4 bg-[#0875e1] hover:bg-[#065cb5] text-white font-semibold py-2 px-4 rounded-lg transition duration-300"
      >
        Publish to LinkedIn
      </button>
      <p v-if="responseMessage" class="mt-4 text-[#0875e1] font-semibold text-center">{{ responseMessage }}</p>
      <p v-if="errorMessage" class="mt-4 text-[#f29c1f] font-semibold text-center">{{ errorMessage }}</p>
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
      responseMessage.value = 'Your post was published successfully!'
      postContent.value = ''
    } else {
      errorMessage.value = 'Oops! Something went wrong.'
    }
  } catch (error) {
    errorMessage.value = 'An unexpected error occurred.'
    console.error(error)
  }
}
</script>

<style>
body {
  background-color: #f7f9fb;
  font-family: 'Helvetica Neue', Arial, sans-serif;
}
</style>
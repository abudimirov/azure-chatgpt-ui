<template>
  <form>
    <div class="mb-3">
      <label for="requestText" class="form-label">Input your request</label>
      <textarea class="form-control" v-model="requestText" rows="3"></textarea>
    </div>
  </form>
  <button @click="getResponse" class="btn btn-primary mb-3">Send</button>
  <div v-if="isLoading">Loading....</div>
  <div v-if="gptResponse" class="response">
    {{ gptResponse }}
  </div>
</template>


<script lang="ts">
import axios from 'axios'

export default {
  name: 'DialogComponent',
  data() {
    return {
      gptResponse: '',
      backendUrl: 'http://localhost:8080/api',
      isLoading: false,
      requestText: null
    }
  },
  methods: {
    async getResponse() {
      this.isLoading = true;
      this.gptResponse = '';
      try {
        const response = await axios.post(this.backendUrl, this.requestText)
        this.gptResponse = response.data
        this.isLoading = false;
      } catch (error) {
        console.log(error);
        this.isLoading = false;
      }
    }
  }
}
</script>

<style scoped></style>

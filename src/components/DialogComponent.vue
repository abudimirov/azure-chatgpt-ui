<template>
  <form>
    <div class="mb-3">
      <label for="requestText" class="form-label">Input your request</label>
      <textarea class="form-control" v-model="requestText" rows="3"></textarea>
    </div>
  </form>
  <button @click="getResponse" class="btn btn-primary mb-5">Send</button>
  <button @click="cleanContext" class="btn btn-danger mb-5">Clean context</button>
  <div v-if="isLoading">Loading....</div>
  <div v-if="gptResponse" class="card">
    <div class="card-body">
      <h6 class="card-subtitle mb-2 text-muted">GPT Response:</h6>
      <div v-html="gptResponse" class="response"></div>
    </div>
  </div>
</template>


<script>
import axios from 'axios'
import { marked } from 'marked'

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
        this.gptResponse = marked(response.data)
        this.isLoading = false;
      } catch (error) {
        console.log(error);
        this.isLoading = false;
      }
    },
    async cleanContext() {
      try {
        await axios.delete(`${this.backendUrl}/clean`)
        this.gptResponse = '';
        alert("Context is flushed!");
      } catch (error) {
        console.log(error);
      }
    }
  }
}
</script>

<style scoped>
pre {
  font-family: Consolas, "Andale Mono WT", "Andale Mono", "Lucida Console", "Lucida Sans Typewriter", "DejaVu Sans Mono", "Bitstream Vera Sans Mono", "Liberation Mono", "Nimbus Mono L", Monaco, "Courier New", Courier, monospace;
  background: #eee;
  padding: 2em;
  white-space: pre-wrap;
  overflow-x: auto; 
}

</style>

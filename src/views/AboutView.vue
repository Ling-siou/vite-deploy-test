<template>
  <div class="about">
    <div>
      <p>{{ content }}</p>
      <QuillEditor v-model:content="content"></QuillEditor>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>

<script>
import { QuillEditor } from '@vueup/vue-quill';
import '@vueup/vue-quill/dist/vue-quill.snow.css';

const url = import.meta.env.VITE_URL;

export default {
  data() {
    return {
      content: '',
    };
  },
  created() {
  },
  components: {
    QuillEditor,
  },
  methods: {
    getTestData() {
      this.$http.get(`${url}comments`).then((response) => {
        console.dir(response.data);
      });
    },
  },
};
</script>

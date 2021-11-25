<template>
  <Card style="margin-top: auto">
    <transition name="fade" mode="out-in">
      <ImageUploadForm v-if="!submitted" @upload-image="onUploadImage" />
      <UploadingLoader v-else />
    </transition>
  </Card>
  <Footer />
</template>

<script lang="ts" setup>
import { ref } from '@vue/reactivity';
import Card from './components/Card.vue';
import ImageUploadForm from './components/ImageUploadForm.vue';
import UploadingLoader from './components/UploadingLoader.vue';
import Footer from './components/Footer.vue';

const submitted = ref<boolean>(false);
const uploaded = ref<boolean>(false);

const onUploadImage = () => submitted.value = true;
</script>

<style>
body {
  margin: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
  background-color: #fafafb;
}

#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  margin: 0 10px;
}

h1 {
  font-size: 1.3rem;
  line-height: 27px;
  font-weight: 500;
  color: #4f4f4f;
  margin-top: 0;
  margin-bottom: 30px;
  text-align: center;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease, max-height 0.5s ease 0.25s;
  opacity: 1;
  max-height: 800px;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  max-height: 50px;
}
</style>

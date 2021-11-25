<template>
  <div>
    <h1>Image Uploader</h1>
    <form @submit.prevent="submit">
      <section @dragover.prevent @drop.prevent="dragAndDroppedImage">
        <label for="image-file">
          <img
            v-if="!uploadedImage"
            src="../assets/image_placeholder.svg"
            alt="Image placeholder"
            height="150"
          />
          <img v-else :src="uploadedImage" alt="Uploaded image" height="150" />
        </label>
        <input
          type="file"
          id="image-file"
          accept="image/*"
          @change="selectedImage"
          hidden
          ref="inputFileRef"
        />
      </section>
      <button type="submit" :disabled="!uploadedImage">Upload</button>
      <button v-if="uploadedImage" type="reset" @click="clear">Clear</button>
    </form>
  </div>
</template>

<script lang="ts" setup>
import { ref } from "vue";

const uploadedImage = ref<string>('');
const inputFileRef = ref<HTMLInputElement | null>(null);

const emit = defineEmits<{
  (event: 'upload-image'): void
}>();

const convertImageToBase64 = async (file: File): Promise<string> =>
  new Promise((resolve, reject) => {
    const fileReader = new FileReader();

    fileReader.readAsDataURL(file);

    fileReader.addEventListener('loadend', (e) => {
      if (!e.target || !e.target.result) {
        reject(false);
      } else {
        resolve(e.target.result.toString());
      }
    })
  })

const verifyImage = (file: File) => file.type.startsWith('image');

const dragAndDroppedImage = async (event: DragEvent) => {
  if (!event.dataTransfer) return;

  const image = event.dataTransfer.files.item(0);

  if (!image) return;

  const isImage = verifyImage(image);

  if (!isImage) return;

  const base64Image = await convertImageToBase64(image);

  uploadedImage.value = base64Image;
}

const selectedImage = async (event: Event) => {
  const uploadedFiles = (event.target as HTMLInputElement).files;

  if (!uploadedFiles) return;

  const image = uploadedFiles.item(0);

  if (!image) return;

  const isImage = verifyImage(image);

  if (!isImage) return;

  const base64Image = await convertImageToBase64(image);

  uploadedImage.value = base64Image;
}

const clear = () => {
  uploadedImage.value = '';

  if (inputFileRef.value) {
    inputFileRef.value.value = ''
  }
}

const submit = () => {
  emit('upload-image')
};
</script>

<style scoped>
form {
  text-align: center;
}

label {
  font-weight: 500;
  font-size: 1rem;
  line-height: 18px;
  color: #646464;
  margin-top: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 30px;
  background-color: #f6f8fb;
  border: 1px dashed #97bef4;
  border-radius: 12px;
  padding: 20px 0;
  cursor: pointer;
}

label span {
  display: block;
}

button {
  background: #2f80ed;
  border-radius: 8px;
  color: white;
  border: none;
  padding: 8px 24px;
  cursor: pointer;
  font-weight: 500;
  font-size: 0.8rem;
  line-height: 16px;
}

button:disabled {
  opacity: 0.5;
  cursor: default;
}

button[type="reset"] {
  background: #ed2f2f;
  margin-left: 1rem;
}
</style>
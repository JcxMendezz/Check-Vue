<template>
  <div class="http-cat-viewer">
    <div class="input-container">
      <input
        v-model="statusCode"
        @input="fetchCatImage"
        placeholder="Ingrese código HTTP"
        type="number"
        min="100"
        max="599"
      />
      <button @click="fetchRandomCat">Aleatorio</button>
    </div>
    <div class="image-container" v-if="imageUrl">
      <img :src="imageUrl" :alt="'HTTP ' + statusCode" @load="imageLoaded" v-show="isImageLoaded" />
      <div class="loader" v-show="!isImageLoaded"></div>
    </div>
    <p class="error" v-if="error">{{ error }}</p>
  </div>
</template>

<script>
import { ref, watch } from 'vue'

export default {
  name: 'HttpCatViewer',
  setup(props, { emit }) {
    const statusCode = ref('')
    const imageUrl = ref('')
    const error = ref('')
    const isImageLoaded = ref(false)

    const fetchCatImage = () => {
      if (statusCode.value && /^[1-5]\d{2}$/.test(statusCode.value)) {
        imageUrl.value = `https://http.cat/${statusCode.value}`
        error.value = ''
        isImageLoaded.value = false
        emit('status-change', statusCode.value)
      } else {
        imageUrl.value = ''
        error.value = 'Por favor, ingrese un código de estado HTTP válido (100-599)'
        emit('status-change', null)
      }
    }

    const fetchRandomCat = () => {
      const randomStatus = Math.floor(Math.random() * (599 - 100 + 1) + 100)
      statusCode.value = randomStatus.toString()
      fetchCatImage()
    }

    const imageLoaded = () => {
      isImageLoaded.value = true
    }

    watch(statusCode, fetchCatImage)

    return {
      statusCode,
      imageUrl,
      error,
      isImageLoaded,
      fetchCatImage,
      fetchRandomCat,
      imageLoaded
    }
  }
}
</script>

<style scoped>
.http-cat-viewer {
  background: rgba(0, 0, 0, 0.7);
  border-radius: 10px;
  padding: 2rem;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
  max-inline-size: 500px;
  inline-size: 100%;
}

.input-container {
  display: flex;
  margin-block-end: 1rem;
}

input {
  flex-grow: 1;
  padding: 0.5rem;
  font-size: 1rem;
  border: none;
  border-radius: 5px 0 0 5px;
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  border: none;
  border-radius: 0 5px 5px 0;
  background: var(--main-color);
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #2980b9;
}

.image-container {
  position: relative;
  inline-size: 100%;
  padding-block-start: 100%; /* 1:1 Aspect Ratio */
  overflow: hidden;
  border-radius: 5px;
}

img {
  position: absolute;
  inset-block-start: 0;
  inset-inline-start: 0;
  inline-size: 100%;
  block-size: 100%;
  object-fit: cover;
  border-radius: 5px;
}

.loader {
  position: absolute;
  inset-block-start: 50%;
  inset-inline-start: 50%;
  transform: translate(-50%, -50%);
  inline-size: 50px;
  block-size: 50px;
  border: 5px solid #f3f3f3;
  border-block-start: 5px solid var(--main-color);
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  100% {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}

.error {
  color: #e74c3c;
  margin-block-start: 1rem;
}
</style>

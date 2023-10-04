<template>
  <div>
    <p class="error">{{ error }}</p>

    <p class="decode-results">
      Last results:
      <ul>
        <li v-for="r in results" :key="r"><b>{{ r }}</b></li>
      </ul>
    </p>

    <!-- <div style="height: 800px; width: 800px;"> -->
      <qrcode-stream @detect="onDetect" @error="onError" />
    <!-- </div> -->
    
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { QrcodeStream } from 'vue-qrcode-reader'

interface ErrorMessages {
  [key: string]: string;
}

const [ results, error ] = [ ref<Array<string>>([]), ref('') ]

const errorMessages: ErrorMessages = {
  NotAllowedError: 'you need to grant camera access permission',
  NotFoundError: 'no camera on this device',
  NotSupportedError: 'secure context required (HTTPS, localhost)',
  NotReadableError: 'is the camera already in use?',
  OverconstrainedError: 'installed cameras are not suitable',
  StreamApiNotSupportedError: 'Stream API is not supported in this browser',
  InsecureContextError: 'Camera access is only permitted in secure context. Use HTTPS or localhost rather than HTTP.'
}

const onDetect = (detectedCodes: [any]) => {
  console.log(detectedCodes)

  for (const code of detectedCodes) {
    results.value.push(code.rawValue)
  }
}

const onError = (err: { name: keyof ErrorMessages; message: any; }) => {
  error.value = `[${err.name}]: ` + (errorMessages[err.name as keyof ErrorMessages] || err.message)
}
</script>

<style scoped>
.error {
  font-weight: bold;
  color: red;
}
</style>

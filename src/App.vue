<template>
  <div>
    <p class="error">{{ error }}</p>

    <p class="decode-result">
      Last result: <b>{{ result }}</b>
    </p>

    <div style="height: 200px; width: 200px;">
      <qrcode-stream @detect="onDetect" @error="onError" />
    </div>
    
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { QrcodeStream } from 'vue-qrcode-reader'

const [ result, error ] = [ ref(''), ref('') ]
  
const errorMessages = {
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

  const [ firstCode ] = detectedCodes
  result.value = firstCode.rawValue
}

const onError = (err: { name: string | number; message: any; }) => {
  error.value = `[${err.name}]: ` + (errorMessages[err.name] || err.message)
}
</script>

<style scoped>
.error {
  font-weight: bold;
  color: red;
}
</style>
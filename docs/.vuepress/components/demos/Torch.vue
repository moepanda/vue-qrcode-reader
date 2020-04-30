<template>
  <div>
    <p class="error" v-if="!torchSupported">
      Torch is not supported on this device.
    </p>

    <qrcode-stream :torch="torchActive" @init="logErrors">
      <button @click="torchActive = !torchActive">
        <img :src="$withBase(icon)" alt="toggle torch">
      </button>
    </qrcode-stream>
  </div>
</template>

<script>
import { QrcodeStream } from 'vue-qrcode-reader'

export default {

  components: { QrcodeStream },

  data () {
    return {
      torchActive: true,
      torchSupported: navigator.mediaDevices.getSupportedConstraints()['torch']
    }
  },

  computed: {
    icon() {
      if (this.torchActive)
        return '/flash-off.svg'
      else
        return '/flash-on.svg'
    }
  },

  methods: {
    logErrors (promise) {
      promise.catch(console.error)
    },

    isTorchSupported() {
      return navigator.mediaDevices.getSupportedConstraints()['torch']
    }
  }
}
</script>

<style scoped>
button {
  position: absolute;
  left: 10px;
  top: 10px;
}

.error {
  color: red;
  font-weight: bold;
}
</style>

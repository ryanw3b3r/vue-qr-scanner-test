<template>
  <div id="app">
    <VueQrScanner @qr-detected="screenIt">
        <main>{{ qr }}</main>
    </VueQrScanner>
  </div>
</template>

<script>
import VueQrScanner from './VueQrScanner.vue'
export default {
    name: 'App',
    components: {
        VueQrScanner
    },
    data() {
        return {
            repeat: 0,
            qr: 'nope',
            word: 0,
            words: ['found', 'yeees', 'got it', 'match', 'scanned'],
        }
    },
    methods: {
        screenIt(response) {
            this.repeat++;
            this.qr = this.repeat + ': ' + this.words[this.word] + ' ' + response.data;
            this.word++;
            if (this.word > 4) this.word = 0;
        }
    },
}
</script>
<style scoped>
main {
    position: absolute;
    font-family: Arial, Helvetica, sans-serif;
    color: #666;
    background: #fff;
    padding: 0.5rem;
    font-size: 1rem;
    left: 0;
    top: 0;
    z-index: 9999;
}
</style>

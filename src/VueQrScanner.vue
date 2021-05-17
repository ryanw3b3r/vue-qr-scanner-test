<template>
    <div class="qr-scannner-container">
        <canvas ref="canvas"></canvas>
        <slot></slot>
    </div>
</template>
<script>
import jsQR from 'jsqr';
export default {
    data() {
        return {
            previous: null,
            video: null
        }
    },
    mounted() {
        this.video = document.createElement('video');

        navigator.mediaDevices
            .getUserMedia({ video: { facingMode: 'environment' } })
            .then(stream => {
                this.video.srcObject = stream;
                this.video.setAttribute('playsinline', true);
                this.video.play();
                requestAnimationFrame(this.tick);
            });
    },
    methods: {
        tick() {
            if (this.video.readyState === this.video.HAVE_ENOUGH_DATA) {
                let canvasEl = this.$refs.canvas;
                let canvas = this.$refs.canvas.getContext('2d');
                
                canvasEl.height = this.video.videoHeight;
                canvasEl.width = this.video.videoWidth;
                canvas.drawImage(this.video, 0, 0, canvasEl.width, canvasEl.height);
                
                const imageData = canvas.getImageData(0, 0, canvasEl.width, canvasEl.height);
                const code = jsQR(imageData.data, imageData.width, imageData.height, { inversionAttempts: 'attemptBoth' });

                if (code && code.data !== this.previous) {
                    this.$emit('qr-detected', { data: code.data, location: code.location });
                    this.previous = code.data;
                }
            }
            requestAnimationFrame(this.tick);
        }
    }
}
</script>
<style scoped>
.qr-scannner-container {
    position: relative;
    width: 100%;
    height: 100%;
}
</style>

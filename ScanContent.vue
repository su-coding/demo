<template>
  <div class="scanner-container">
    <qrcode-stream
        @detect="onDetect"
        :track="paintOutline"
        :formats="['qr_code','code_128']"
    />
    <div class="result" v-if="result">
      扫描结果: <b>{{ result }}</b>
    </div>
  </div>
</template>

<script>
import { QrcodeStream } from 'vue-qrcode-reader'

export default {
  components: {
    QrcodeStream
  },
  data() {
    return {
      result: '',
      error: '',
      detectedCount: 0,
      lastDetection: ''
    }
  },
  methods: {
    onDetect(detectedCodes) {
      this.detectedCount = detectedCodes.length
      if (detectedCodes.length > 0) {
        this.result = detectedCodes[0].rawValue
      }
    },

    paintOutline(detectedCodes, ctx) {
      for (const detectedCode of detectedCodes) {
        const [ firstPoint, ...otherPoints ] = detectedCode.cornerPoints

        ctx.strokeStyle = '#00ff00'
        ctx.lineWidth = 3
        ctx.setLineDash([])

        ctx.beginPath()
        ctx.moveTo(firstPoint.x, firstPoint.y)
        for (const { x, y } of otherPoints) {
          ctx.lineTo(x, y)
        }
        ctx.lineTo(firstPoint.x, firstPoint.y)
        ctx.closePath()
        ctx.stroke()

        ctx.fillStyle = '#00ff00'
        ctx.font = 'bold 14px Arial'
        ctx.fillText(`${detectedCode.format} Detected`, firstPoint.x, firstPoint.y - 10)
      }
    }
  }
}
</script>

<style scoped>
.scanner-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.result {
  margin: 20px 0;
  padding: 15px;
  background: #f0f8ff;
  border: 1px solid #b3d9ff;
  border-radius: 4px;
  word-break: break-all;
}
</style>
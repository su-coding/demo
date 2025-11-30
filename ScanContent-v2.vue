<template>
  <div class="scanner-container">
    <qrcode-stream
        @detect="onDetect"
        :track="paintOutline"
        :formats="['qr_code','code_128']"
    />
    <!-- 绿色扫描框 -->
    <div class="scan-frame">
      <div class="corner top-left"></div>
      <div class="corner top-right"></div>
      <div class="corner bottom-left"></div>
      <div class="corner bottom-right"></div>
      <!-- 扫描线 -->
      <div class="scan-line"></div>
    </div>
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
  position: relative;
}

.result {
  margin: 20px 0;
  padding: 15px;
  background: #f0f8ff;
  border: 1px solid #b3d9ff;
  border-radius: 4px;
  word-break: break-all;
}

/* 扫描框样式 */
.scan-frame {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 300px;
  height: 200px;
  background: transparent;
  z-index: 10;
  pointer-events: none;
}

/* 四个角 */
.corner {
  position: absolute;
  width: 25px;
  height: 25px;
  border: 3px solid #00ff00;
  background: transparent;
}

.corner.top-left {
  top: -3px;
  left: -3px;
  border-right: none;
  border-bottom: none;
}

.corner.top-right {
  top: -3px;
  right: -3px;
  border-left: none;
  border-bottom: none;
}

.corner.bottom-left {
  bottom: -3px;
  left: -3px;
  border-right: none;
  border-top: none;
}

.corner.bottom-right {
  bottom: -3px;
  right: -3px;
  border-left: none;
  border-top: none;
}

/* 扫描线 */
.scan-line {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 3px;
  background: linear-gradient(90deg, transparent 0%, #00ff00 50%, transparent 100%);
  animation: scan 2s linear infinite;
  box-shadow: 0 0 10px #00ff00;
}

@keyframes scan {
  0% {
    top: 0;
    opacity: 1;
  }
  50% {
    top: calc(100% - 3px);
    opacity: 0.8;
  }
  100% {
    top: 0;
    opacity: 1;
  }
}
</style>
<template>
  <div class="webcontent">
    <span>The powerful Iran-backed group vowed to respond. The threat triggered a slew of flight cancelations on both sides of the border, a chorus of governments imploring their citizens</span><br/>
    <span>The powerful Iran-backed group vowed to respond. The threat triggered a slew of flight cancelations on both sides of the border, a chorus of governments imploring their citizens</span><br/>
    <span>The powerful Iran-backed group vowed to respond. The threat triggered a slew of flight cancelations on both sides of the border, a chorus of governments imploring their citizens</span><br/>
    <span>The powerful Iran-backed group vowed to respond. The threat triggered a slew of flight cancelations on both sides of the border, a chorus of governments imploring their citizens</span><br/>
    <span>The powerful Iran-backed group vowed to respond. The threat triggered a slew of flight cancelations on both sides of the border, a chorus of governments imploring their citizens</span><br/>
    <span>The powerful Iran-backed group vowed to respond. The threat triggered a slew of flight cancelations on both sides of the border, a chorus of governments imploring their citizens</span><br/>
    <span>The powerful Iran-backed group vowed to respond. The threat triggered a slew of flight cancelations on both sides of the border, a chorus of governments imploring their citizens</span><br/>
    <span>The powerful Iran-backed group vowed to respond. The threat triggered a slew of flight cancelations on both sides of the border, a chorus of governments imploring their citizens</span><br/>
    <span>111111111111111111111111111111111111111111111111111111111111111111111</span><br/>
    <span>111111111111111111111111111111111111111111111111111111111111111111111</span><br/>
    <span>111111111111111111111111111111111111111111111111111111111111111111111</span><br/>
  </div>
  <canvas ref="watermarkCanvas" class="watermarkCanvas"></canvas>
</template>

<script>
import axios from 'axios';

export default {
  name: 'WaterMask',
  data() {
    return {
      publicIP: 'loading IP...',
      currentTime: '',
    };
  },
  mounted() {
    this.getPublicIP();
    this.updateTime();
    setInterval(this.updateTime, 1000);
    this.drawWatermark();
    this.observeCanvas();
  },
  methods: {
    async getPublicIP() {
      try {
        const res = await axios.get('https://api.ipify.org?format=json');
        this.publicIP = res.data.ip;
        this.drawWatermark();
      } catch (error) {
        console.error('cat not get IP：', error);
        this.publicIP = 'get IP failed';
        this.drawWatermark();
      }
    },
    updateTime() {
      const now = new Date();
      const options = { timeZone: 'Asia/Tokyo', hour12: false };
      this.currentTime = now.toLocaleTimeString('ja-JP', options);
      this.drawWatermark();
    },
    drawWatermark() {
      const canvas = this.$refs.watermarkCanvas;
      const ctx = canvas.getContext('2d');
      const text = `Login_User: admin  |  IP: ${this.publicIP}  |  CurrentTime: ${this.currentTime}`;
      const fontSize = 20;
      const padding = 50;

      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;

      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.font = `${fontSize}px Arial`;
      ctx.fillStyle = 'rgba(0, 0, 0, 0.2)';
      ctx.rotate(-Math.PI / 6); // 旋转 -30 度

      for (let y = padding; y < canvas.height; y += padding + fontSize) {
        for (let x = -canvas.width; x < canvas.width * 2; x += ctx.measureText(text).width + padding) {
          ctx.fillText(text, x, y);
        }
      }

      ctx.rotate(Math.PI / 6); // 恢复旋转角度
    },
    observeCanvas() {
      const canvas = this.$refs.watermarkCanvas;
      const observer = new MutationObserver((mutations) => {
        mutations.forEach((mutation) => {
          if (mutation.removedNodes.length) {
            mutation.removedNodes.forEach((node) => {
              if (node === canvas) {
                console.warn('Watermark canvas has been removed, re-adding it.');
                document.body.appendChild(canvas); // 重新添加 canvas 元素
                this.drawWatermark(); // 重新绘制水印
              }
            });
          }
        });
      });

      observer.observe(document.body, {
        childList: true,
        subtree: true,
      });
    },
  },
};
</script>

<style scoped>
.webcontent {
  position: relative;
  padding: 20px;
  margin: 50px;
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
  font-size: 35px;
  text-align: right;
  z-index: 1;
}

.watermarkCanvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 999999999999;
  pointer-events: none;
}
</style>

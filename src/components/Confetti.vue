<template>
  <div class="confetti-container" ref="confettiRef">
    <canvas ref="canvas" class="confetti-canvas"></canvas>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";

// 定义组件名
defineOptions({
  name: "ConfettiEffect",
});

interface ConfettiPiece {
  x: number;
  y: number;
  vx: number;
  vy: number;
  rotation: number;
  rotationSpeed: number;
  color: string;
  size: number;
  opacity: number;
  life: number;
  maxLife: number;
}

const canvas = ref<HTMLCanvasElement>();
const confettiRef = ref<HTMLElement>();
let animationId: number | null = null;
let confettiPieces: ConfettiPiece[] = [];

const colors = [
  "#ff6b6b",
  "#4ecdc4",
  "#45b7d1",
  "#96ceb4",
  "#feca57",
  "#ff9ff3",
  "#54a0ff",
  "#5f27cd",
  "#00d2d3",
  "#ff9f43",
  "#ff9ff3",
  "#54a0ff",
  "#5f27cd",
  "#00d2d3",
  "#ff9f43",
];

const fireConfetti = () => {
  if (!canvas.value) return;

  const rect = confettiRef.value?.getBoundingClientRect();
  if (!rect) return;

  // 从容器中心发射礼花
  const centerX = rect.width / 2;
  const centerY = rect.height / 2;

  // 创建新的礼花碎片
  for (let i = 0; i < 50; i++) {
    const angle = (Math.PI * 2 * i) / 50;
    const velocity = Math.random() * 8 + 4;

    confettiPieces.push({
      x: centerX,
      y: centerY,
      vx: Math.cos(angle) * velocity,
      vy: Math.sin(angle) * velocity - 2,
      rotation: Math.random() * 360,
      rotationSpeed: (Math.random() - 0.5) * 20,
      color: colors[Math.floor(Math.random() * colors.length)],
      size: Math.random() * 6 + 3,
      opacity: 1,
      life: 0,
      maxLife: Math.random() * 100 + 100,
    });
  }
};

// 持续播放礼花效果
const startContinuousConfetti = () => {
  const interval = setInterval(() => {
    fireConfetti();
  }, 2000); // 每2秒发射一次礼花

  // 保存interval ID以便清理
  return interval;
};

const animate = () => {
  if (!canvas.value) return;

  const ctx = canvas.value.getContext("2d");
  if (!ctx) return;

  // 清除画布
  ctx.clearRect(0, 0, canvas.value.width, canvas.value.height);

  // 更新和绘制礼花碎片
  confettiPieces.forEach((piece, index) => {
    // 更新位置
    piece.x += piece.vx;
    piece.y += piece.vy;
    piece.rotation += piece.rotationSpeed;
    piece.life++;

    // 添加重力
    piece.vy += 0.15;

    // 计算透明度
    piece.opacity = Math.max(0, 1 - piece.life / piece.maxLife);

    // 绘制碎片
    ctx.save();
    ctx.translate(piece.x, piece.y);
    ctx.rotate((piece.rotation * Math.PI) / 180);
    ctx.globalAlpha = piece.opacity;

    // 绘制不同形状的碎片
    const shape = Math.floor(Math.random() * 3);
    ctx.fillStyle = piece.color;

    switch (shape) {
      case 0: // 方形
        ctx.fillRect(-piece.size / 2, -piece.size / 2, piece.size, piece.size);
        break;
      case 1: // 圆形
        ctx.beginPath();
        ctx.arc(0, 0, piece.size / 2, 0, Math.PI * 2);
        ctx.fill();
        break;
      case 2: // 三角形
        ctx.beginPath();
        ctx.moveTo(0, -piece.size / 2);
        ctx.lineTo(-piece.size / 2, piece.size / 2);
        ctx.lineTo(piece.size / 2, piece.size / 2);
        ctx.closePath();
        ctx.fill();
        break;
    }

    ctx.restore();

    // 移除生命周期结束的碎片
    if (piece.life >= piece.maxLife) {
      confettiPieces.splice(index, 1);
    }
  });

  animationId = requestAnimationFrame(animate);
};

const handleResize = () => {
  if (canvas.value && confettiRef.value) {
    const rect = confettiRef.value.getBoundingClientRect();
    canvas.value.width = rect.width;
    canvas.value.height = rect.height;
  }
};

let continuousInterval: number | null = null;

onMounted(() => {
  handleResize();
  animate();

  // 启动持续播放
  continuousInterval = startContinuousConfetti();

  window.addEventListener("resize", handleResize);
});

onUnmounted(() => {
  if (animationId) {
    cancelAnimationFrame(animationId);
  }
  if (continuousInterval) {
    clearInterval(continuousInterval);
  }
  window.removeEventListener("resize", handleResize);
});

// 暴露fire方法
defineExpose({
  fire: fireConfetti,
});
</script>

<style scoped>
.confetti-container {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  overflow: hidden;
  border-radius: inherit;
}

.confetti-canvas {
  display: block;
  width: 100%;
  height: 100%;
}
</style>

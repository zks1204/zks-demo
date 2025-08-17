<template>
  <!-- 卡片中间 - 突出显示图片 -->
  <div class="card-center">
    <div class="card-container">
      <img src="@/assets/image.png" alt="Center Image" class="card-image" />
    </div>

    <!-- 操作按钮区域 -->
    <div class="action-buttons">
      <button class="btn btn-share">Share On Twitter</button>
      <button class="btn btn-download">Download Picture</button>
    </div>
  </div>
</template>

<script setup lang="ts">
</script>

<style scoped>
@import "../assets/share.css";

.card-center {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
  border-radius: 32px;
  height: 60vh;
  background-color: white;
  padding: 25px;
}

.card-container {
  background-color: #e4dccf;
  border-radius: 20px;
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 100%;
  margin: 0px 0px;
  height: 80%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.card-image {
  width: 60%;
  height: auto;
  border-radius: 8px;
  display: block;
  margin: 0 auto;
  box-shadow: 0 15px 20px rgba(239, 58, 30, 0.4);
  transition: all 0.3s ease;
}

.card-image:hover {
  box-shadow: 0 20px 30px rgba(239, 58, 30, 0.6);
  transform: scale(1.02);
}

.action-buttons {
  display: flex;
  gap: 30px;
  height: 20%;
  align-items: center;
  justify-content: center;
}

.btn-share {
  background: #495057;
  color: white;
}

.btn-download {
  background: white;
  color: #343a40;
  border: black solid 1px;
}
</style>

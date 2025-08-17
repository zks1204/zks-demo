<template>
  <div class="share-container">
    <div class="content-container">
      <!-- 标题区域 -->
      <div class="header-section margin-bottom-content">
        <div class="title-container" @mouseenter="fireConfetti">
          <h1 class="congratulations">Congratulations!</h1>
          <h2 class="title">Early Access Account Created</h2>
          <Confetti ref="confettiRef" />
        </div>
      </div>
      <div class="margin-bottom-content">
        <ShareCard />
      </div>

      <!-- 底部区域 -->
      <div class="bottom-section margin-bottom-content">
        <button class="btn btn-continue">Continue →</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Confetti from "@/components/Confetti.vue";
import ShareCard from "@/components/ShareCard.vue";

const confettiRef = ref();

const fireConfetti = () => {
  confettiRef.value?.fire();
};
</script>

<style scoped>
@import "../assets/share.css";

.share-container {
  width: 100%;
  height: 100%;
  background: linear-gradient(
    135deg,
    #f6ece3 0%,
    #eeefee 25%,
    #f6efea 50%,
    #efeeeb 100%
  );
  margin: 0;
  padding: 0;
  /* overflow: hidden; */
  display: flex;
  justify-content: center;
  align-items: center;
}

.content-container {
  width: 40%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.header-section {
  text-align: center;
}

.margin-bottom-content {
  margin-bottom: 40px;
}

.title-container {
  display: inline-block;
  padding: 20px;
  cursor: pointer;
}

.title-container:hover {
  transform: translateY(-2px);
}

.congratulations {
  font-size: 24px;
  color: #6c757d;
  margin: 16px 0;
  font-weight: normal;
}

.title {
  font-size: 32px;
  font-weight: bold;
  color: #343a40;
  margin: 0;
}

.bottom-section {
  display: flex;
  align-self: flex-end;
}

.btn-continue {
  background: #495057;
  color: white;
}

.btn-continue:hover {
  background: #343a40;
  transform: translateY(-2px);
}
</style>

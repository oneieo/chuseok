<template>
  <div class="chuseok-container">
    <header class="header">
      <h1>풍요로운 한가위</h1>
      <h2>소원 비는 보름달</h2>
    </header>

    <main class="main-content">
      <div class="moon-wrapper">
        <div class="moon"></div>
        <!-- 달 표면의 그림자를 CSS로 간단히 표현 -->
        <div class="moon-shadow"></div>
      </div>
      <p class="greeting">{{ greetingMessage }}</p>
    </main>

    <footer class="footer">
      <p>가족과 함께 즐겁고 넉넉한 시간 보내세요.</p>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

// 한가위 인사말
const greetingMessage = ref<string>(
  "보름달처럼 넉넉하고 풍요로운 마음으로, 즐거운 추석 연휴 보내시길 기원합니다."
);
</script>

<style scoped>
@import url("https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css");

/* 메인 컨테이너 스타일 */
.chuseok-container {
  font-family: "Pretendard", sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  width: 100%;
  /* 밤하늘 배경색 */
  background: linear-gradient(to bottom, #0a0a23, #1a1a3d);
  /* 따뜻한 느낌의 텍스트 색상 */
  color: #f0e6d2;
  padding: 2rem;
  box-sizing: border-box;
  text-align: center;
}

/* 헤더 스타일 */
.header h1 {
  font-size: 3rem;
  font-weight: 700;
  margin: 0;
  color: #fff;
  text-shadow: 0 0 10px #fdf6e4;
}

.header h2 {
  font-size: 1.5rem;
  font-weight: 400;
  margin-top: 0.5rem;
  color: #d0c0a0;
}

/* 메인 콘텐츠 영역 */
.main-content {
  margin: 3rem 0;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* 달을 감싸는 래퍼 (애니메이션 효과) */
.moon-wrapper {
  position: relative;
  /* 은은하게 빛나는 애니메이션 */
  animation: gentle-glow 4s ease-in-out infinite;
}

/* 보름달 스타일 */
.moon {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background-color: #fdf6e4;
  /* 달빛이 퍼지는 효과 */
  box-shadow: 0 0 25px #fdf6e4, 0 0 50px #fdf6e4, 0 0 75px #fff;
  position: relative;
  overflow: hidden; /* 그림자가 밖으로 나가지 않도록 */
}

/* 달 표면 그림자 (간단한 표현) */
.moon-shadow {
  position: absolute;
  top: 25%;
  left: 15%;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, 0.08);
  box-shadow: 15px 30px 0 rgba(0, 0, 0, 0.08),
    40px -10px 0 20px rgba(0, 0, 0, 0.06);
}

/* 인사말 스타일 */
.greeting {
  font-size: 1.25rem;
  line-height: 1.8;
  margin-top: 3rem;
  max-width: 600px;
}

/* 푸터 스타일 */
.footer {
  font-size: 1rem;
  color: #d0c0a0;
  margin-top: 2rem;
}

/* 달빛 애니메이션 */
@keyframes gentle-glow {
  0% {
    transform: scale(1);
    opacity: 0.9;
  }
  50% {
    transform: scale(1.03);
    opacity: 1;
  }
  100% {
    transform: scale(1);
    opacity: 0.9;
  }
}

/* 모바일 반응형 스타일 */
@media (max-width: 600px) {
  .header h1 {
    font-size: 2.2rem;
  }
  .header h2 {
    font-size: 1.2rem;
  }
  .moon {
    width: 150px;
    height: 150px;
  }
  .greeting {
    font-size: 1.1rem;
    margin-top: 2rem;
  }
}
</style>

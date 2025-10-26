<template>
  <div class="chuseok-container">
    <header class="header">
      <h1>풍요로운 한가위</h1>
      <h2>소원 비는 보름달</h2>
    </header>

    <!-- 
        Vue의 <Transition> 컴포넌트를 사용해
        'main', 'food', 'game' 뷰가 전환될 때 부드러운 fade 효과를 줍니다.
      -->
    <Transition name="fade" mode="out-in">
      <!-- 1. 메인 뷰 (보름달, 소원 빌기) -->
      <main class="main-content" v-if="activeView === 'main'" key="main">
        <div class="moon-wrapper">
          <div class="moon"></div>
          <div class="moon-shadow"></div>
        </div>
        <p class="greeting">{{ greetingMessage }}</p>

        <!-- '소원 빌기' 버튼 -->
        <button @click="makeAWish" class="wish-button">소원 빌기</button>

        <!-- 컨텐츠 네비게이션 버튼 -->
        <div class="nav-buttons">
          <button @click="activeView = 'food'" class="nav-button">
            한가위 음식
          </button>
          <button @click="activeView = 'game'" class="nav-button">
            전통 풍습
          </button>
        </div>
      </main>

      <!-- 2. 한가위 음식 뷰 -->
      <section
        class="content-section"
        v-else-if="activeView === 'food'"
        key="food"
      >
        <h2>추석의 대표 음식</h2>
        <div class="card-grid" style="grid-template-columns: repeat(2, 1fr)">
          <!-- 송편 카드 -->
          <div class="info-card">
            <img
              src="https://placehold.co/400x300/5a5a7d/f0e6d2?text=송편"
              alt="송편 이미지"
              class="card-img"
              @error="onImgError"
            />
            <h3>송편 (Songpyeon)</h3>
            <p>
              그 해 수확한 햅쌀로 빚어 조상께 감사하는 마음을 담은, 한가위의
              상징적인 떡입니다.
            </p>
          </div>
          <!-- 전 카드 -->
          <div class="info-card">
            <img
              src="https://placehold.co/400x300/5a5a7d/f0e6d2?text=각종+전"
              alt="각종 전 이미지"
              class="card-img"
              @error="onImgError"
            />
            <h3>각종 전 (Jeon)</h3>
            <p>
              동그랑땡, 꼬치전, 동태전 등 기름에 지진 전은 명절의 '풍요로움'을
              상징합니다.
            </p>
          </div>
        </div>
        <button @click="activeView = 'main'" class="back-button">
          돌아가기
        </button>
      </section>

      <!-- 3. 전통 풍습 뷰 -->
      <section
        class="content-section"
        v-else-if="activeView === 'game'"
        key="game"
      >
        <h2>한가위 전통 풍습</h2>
        <div class="card-grid" style="grid-template-columns: repeat(3, 1fr)">
          <!-- 강강술래 카드 -->
          <div class="info-card">
            <img
              src="https://placehold.co/400x300/5a5a7d/f0e6d2?text=강강술래"
              alt="강강술래 이미지"
              class="card-img"
              @error="onImgError"
            />
            <h3>강강술래</h3>
            <p>
              밝은 보름달 아래, 여성들이 손을 잡고 원을 그리며 풍요와 안녕을
              기원하던 놀이입니다.
            </p>
          </div>
          <!-- 윷놀이 카드 -->
          <div class="info-card">
            <img
              src="https://placehold.co/400x300/5a5a7d/f0e6d2?text=윷놀이"
              alt="윷놀이 이미지"
              class="card-img"
              @error="onImgError"
            />
            <h3>윷놀이</h3>
            <p>
              가족, 친지들이 모여 편을 나누어 즐기는 대표적인 민속놀이로, 화합을
              다집니다.
            </p>
          </div>
          <!-- 차례 카드 -->
          <div class="info-card">
            <img
              src="https://placehold.co/400x300/5a5a7d/f0e6d2?text=차례"
              alt="차례 이미지"
              class="card-img"
              @error="onImgError"
            />
            <h3>차례 (Charye)</h3>
            <p>
              햇곡식과 햇과일로 정성껏 상을 차려 조상께 감사를 전하는 중요한
              의식입니다.
            </p>
          </div>
        </div>
        <button @click="activeView = 'main'" class="back-button">
          돌아가기
        </button>
      </section>
    </Transition>

    <footer class="footer">
      <p>가족과 함께 즐겁고 넉넉한 시간 보내세요.</p>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

// 현재 보여줄 뷰를 관리하는 상태 ('main', 'food', 'game')
const activeView = ref<"main" | "food" | "game">("main");

// 한가위 인사말 (초기 메시지)
const greetingMessage = ref<string>(
  "보름달처럼 넉넉하고 풍요로운 마음으로,\n즐거운 추석 연휴 보내시길 기원합니다."
);

// 소원 메시지 목록
const wishes: string[] = [
  "당신의 소원이 꼭 이루어지길 바랍니다.",
  "늘 건강하고 행복하세요!",
  "웃음꽃 가득한 명절 보내세요.",
  "보름달처럼 환한 일만 가득하길!",
];

// 소원 빌기 함수
const makeAWish = () => {
  // 현재 메시지와 다른 메시지를 뽑도록 필터링
  const otherWishes = wishes.filter((w) => w !== greetingMessage.value);

  let pool = otherWishes.length > 0 ? otherWishes : wishes;

  const randomIndex = Math.floor(Math.random() * pool.length);
  greetingMessage.value = pool[randomIndex] as string;
};

// 이미지 에러 핸들러 (플레이스홀더용)
const onImgError = (event: Event) => {
  const target = event.target as HTMLImageElement;
  target.src =
    "https://placehold.co/400x300/5a5a7d/f0e6d2?text=이미지+로드+실패";
};
</script>

<style scoped>
/* 프리텐다드 웹폰트 임포트 (CDN) */
@import url("https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css");

/* 전역 스타일 초기화 및 기본 설정 */
:global(body, html) {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow-x: hidden;
}

:global(#app) {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* 메인 컨테이너 스타일 */
.chuseok-container {
  font-family: "Pretendard", sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  width: 100%;
  background: linear-gradient(to bottom, #0a0a23, #1a1a3d);
  color: #f0e6d2;
  padding: 2rem;
  box-sizing: border-box;
  text-align: center;
  overflow-x: hidden; /* 가로 스크롤 방지 */
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

/* --- 메인 컨텐츠 영역 (공통) --- */
.main-content,
.content-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 900px; /* 컨텐츠 최대 너비 제한 */
  margin: 3rem 0;
}

/* --- 트랜지션(Fade) 효과 --- */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* --- 메인 뷰 --- */
.moon-wrapper {
  position: relative;
  animation: gentle-glow 4s ease-in-out infinite;
}

.moon {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background-color: #fdf6e4;
  box-shadow: 0 0 25px #fdf6e4, 0 0 50px #fdf6e4, 0 0 75px #fff;
  position: relative;
  overflow: hidden;
}

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

.greeting {
  font-size: 1.25rem;
  line-height: 1.8;
  margin-top: 3rem;
  max-width: 600px;
  white-space: pre-line; /* \n 줄바꿈 적용 */
  transition: opacity 0.3s ease-in-out;
}

.wish-button {
  font-family: "Pretendard", sans-serif;
  font-size: 1rem;
  font-weight: 600;
  color: #0a0a23;
  background-color: #f0e6d2;
  border: none;
  border-radius: 50px;
  padding: 0.8rem 1.8rem;
  margin-top: 2rem;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(240, 230, 210, 0.2);
}

.wish-button:hover {
  transform: translateY(-2px);
  background-color: #fff;
  box-shadow: 0 6px 20px rgba(240, 230, 210, 0.3);
}

.wish-button:active {
  transform: translateY(0);
}

/* 네비게이션 버튼 (신규) */
.nav-buttons {
  display: flex;
  gap: 1rem;
  margin-top: 2.5rem;
}

.nav-button {
  font-family: "Pretendard", sans-serif;
  font-size: 0.9rem;
  font-weight: 500;
  color: #f0e6d2;
  background-color: transparent;
  border: 1px solid #f0e6d2;
  border-radius: 50px;
  padding: 0.6rem 1.2rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.nav-button:hover {
  background-color: rgba(240, 230, 210, 0.1);
  transform: translateY(-2px);
}

/* --- 컨텐츠 섹션 (음식, 풍습) --- */
.content-section h2 {
  font-size: 2.2rem;
  font-weight: 700;
  color: #fff;
  margin-bottom: 2.5rem;
}

/* 카드 그리드 (신규) */
.card-grid {
  display: grid;
  gap: 1.5rem;
  width: 100%;
}

/* 컨텐츠 카드 (신규) */
.info-card {
  /* Glassmorphism 스타일 */
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  padding: 1.5rem;
  text-align: left;
  backdrop-filter: blur(5px);
  -webkit-backdrop-filter: blur(5px);
  color: #f0e6d2;
  transition: all 0.3s ease;
}

.info-card:hover {
  transform: translateY(-5px);
  background: rgba(255, 255, 255, 0.08);
}

.card-img {
  width: 100%;
  height: auto;
  aspect-ratio: 16 / 9;
  object-fit: cover;
  border-radius: 10px;
  margin-bottom: 1rem;
  background-color: #5a5a7d; /* 플레이스홀더 배경 */
}

.info-card h3 {
  font-size: 1.5rem;
  font-weight: 700;
  color: #fff;
  margin-top: 0;
  margin-bottom: 0.5rem;
}

.info-card p {
  font-size: 1rem;
  line-height: 1.6;
}

/* 돌아가기 버튼 (신규) */
.back-button {
  /* .wish-button과 동일한 스타일 적용 */
  font-family: "Pretendard", sans-serif;
  font-size: 1rem;
  font-weight: 600;
  color: #0a0a23;
  background-color: #f0e6d2;
  border: none;
  border-radius: 50px;
  padding: 0.8rem 1.8rem;
  margin-top: 2.5rem;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(240, 230, 210, 0.2);
}

.back-button:hover {
  transform: translateY(-2px);
  background-color: #fff;
  box-shadow: 0 6px 20px rgba(240, 230, 210, 0.3);
}

/* --- 푸터 --- */
.footer {
  font-size: 1rem;
  color: #d0c0a0;
  margin-top: 2rem;
  /* 뷰 전환과 관계없이 항상 보이도록 main/section 밖으로 이동 */
}

/* --- 달빛 애니메이션 --- */
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

/* --- 모바일 반응형 스타일 --- */
@media (max-width: 768px) {
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
  .wish-button {
    font-size: 0.9rem;
    padding: 0.7rem 1.5rem;
  }
  .nav-buttons {
    flex-direction: column;
    gap: 0.5rem;
  }
  .nav-button {
    font-size: 0.9rem;
    padding: 0.6rem 1.2rem;
  }

  /* 모바일에선 카드 그리드 1열로 */
  .card-grid {
    grid-template-columns: 1fr !important; /* 인라인 스타일 덮어쓰기 */
  }
  .content-section h2 {
    font-size: 1.8rem;
  }
  .info-card h3 {
    font-size: 1.3rem;
  }
  .info-card p {
    font-size: 0.95rem;
  }
  .back-button {
    margin-top: 2rem;
  }
}
</style>

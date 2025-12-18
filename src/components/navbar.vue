<template>
  <header class="app-header">
    <h1 class="title">折枝电子设定集</h1>
    <!-- 在线人数展示 -->
    <div class="online-count" v-if="onlineCount !== null">
      当前在线：<span class="count">{{ onlineCount }}人</span>
    </div>
    <!-- 移动端汉堡按钮 -->
    <button
      class="hamburger"
      @click="toggleMobileNav"
      aria-label="Toggle navigation"
    >
      <span :class="{ open: mobileNavOpen }"></span>
      <span :class="{ open: mobileNavOpen }"></span>
      <span :class="{ open: mobileNavOpen }"></span>
    </button>

    <!-- 普通导航 & 移动端下拉导航 -->
    <nav :class="['nav-links', { 'mobile-open': mobileNavOpen }]">
      <RouterLink
        v-for="item in navItems"
        :key="item.name"
        :to="item.path"
        class="nav-item"
        active-class="active-link"
        @click="mobileNavOpen = false"
      >
        {{ item.name }}
      </RouterLink>

      <a
        href="http://36.150.237.25/#/redirector"
        target="_blank"
        rel="noopener"
        class="nav-item"
        active-class="active-link"
        @click="mobileNavOpen = false"
      >
        霜落映界
      </a>
    </nav>
  </header>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from "vue";
import { io } from "socket.io-client";

const navItems = [
  { name: "浮墨绘心", path: "/" },
  { name: "丹青时迹", path: "/timeLine" },
  { name: "墨语回声", path: "/message" },
  { name: "灵犀画境", path: "/gallery" },
  { name: "万象墨藏", path: "/resources" },
  { name: "瑝珑笔叙", path: "/talk" },
  { name: "鹤影声痕", path: "/voice" },
  { name: "和鸣墨韵", path: "/music" },
  { name: "绘世白簿", path: "/wiki" },
  { name: "千鹤梦回", path: "/thanks" },
];

const mobileNavOpen = ref(false);
function toggleMobileNav() {
  mobileNavOpen.value = !mobileNavOpen.value;
}

const siteId = "zhezhi";

const onlineCount = ref<number | null>(null);

// 连接时带上 query.siteId
const socket: any = io("http://36.150.237.25:3000", {
  query: { siteId },
});

onMounted(() => {
  socket.on("onlineCount", (count: number) => {
    onlineCount.value = count;
  });
});

onBeforeUnmount(() => {
  socket.disconnect();
});
</script>

<style scoped lang="scss">
/* 折枝 — 墨笔丹青：宣纸底 / 墨色冷凝 / 朱砂点缀 + 鹤影笔触 */
.app-header {
  /* 语义化变量：宣纸、墨色、点缀、墨羽 */
  --paper-bg-start: #f6f3ee; /* 宣纸高光 */
  --paper-bg-end: #efe7dc; /* 宣纸暗调 */
  --ink-core: #0c0f12; /* 墨色深核 */
  --ink-wash: rgba(12, 14, 18, 0.06); /* 墨渍薄洗 */
  --accent: #2e8aa3; /* 服饰主色（青/湖） */
  --accent-2: #f3a6b8; /* 次色（粉/朱） */
  --seal-red: #b94b4b; /* 朱砂点缀 */
  --muted-text: #1b1d20; /* 宣纸上较深的文字色 */
  --shadow-core: rgba(4, 6, 10, 0.36);

  position: fixed;
  inset: 0 0 auto 0;
  z-index: 1000;
  height: 64px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 40px;
  /* 宣纸纹理感 + 轻微墨水晕染 */
  background: radial-gradient(
      420px 100px at 14% 22%,
      rgba(46, 138, 160, 0.03),
      transparent 12%
    ),
    linear-gradient(180deg, var(--paper-bg-start) 0%, var(--paper-bg-end) 100%);
  backdrop-filter: blur(4px) saturate(0.98);
  box-shadow: 0 8px 22px var(--shadow-core),
    0 0 8px rgba(10, 10, 10, 0.02) inset;
  border-bottom: 1px solid rgba(10, 12, 14, 0.04);
  animation: fadeInDown 0.45s ease-out both;
  overflow: visible;
  .title {
    position: relative;
    font-family: "Noto Serif SC", "STKaiti", serif;
    font-size: 26px;
    font-weight: 800;
    color: var(--muted-text);
    /* 文字以墨色为主，内含一点青色渐变作为"颜料"痕迹 */
    background: linear-gradient(90deg, var(--ink-core), var(--accent));
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    letter-spacing: 0.4px;
    text-shadow: 0 6px 18px rgba(4, 6, 8, 0.28);
    transition: transform 0.26s ease, text-shadow 0.26s ease;
    animation: brush-pulse 8s ease-in-out infinite;

    &:hover {
      transform: translateY(-2px) scale(1.02);
      text-shadow: 0 12px 28px rgba(20, 30, 34, 0.06);
    }
  }

  .online-count {
    position: relative;
    margin-left: 16px;
    padding: 6px 14px;
    font-family: "Noto Sans SC", "PingFang SC", sans-serif;
    font-size: 1rem;
    color: var(--muted-text);
    background: linear-gradient(
      135deg,
      rgba(255, 255, 255, 0.82),
      rgba(250, 248, 244, 0.92)
    );
    border: 1px solid rgba(10, 10, 10, 0.04);
    border-radius: 22px;
    backdrop-filter: blur(4px);
    box-shadow: 0 6px 12px rgba(3, 6, 10, 0.06),
      0 0 8px rgba(255, 255, 255, 0.02) inset;
    overflow: hidden;
    cursor: default;
    transition: transform 0.22s ease, box-shadow 0.22s ease;

    &:hover {
      transform: translateY(-2px);
      box-shadow: 0 12px 28px rgba(3, 6, 10, 0.12);
    }

    .count {
      display: inline-block;
      margin-left: 18px;
      font-weight: 800;
      color: var(--accent-2);
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      text-shadow: 0 0 6px rgba(243, 166, 184, 0.02);
    }
  }

  .nav-links {
    display: flex;
    gap: 22px;
    align-items: center;

    .nav-item {
      position: relative;
      color: var(--muted-text);
      font-weight: 600;
      text-decoration: none;
      transition: color 0.22s ease, transform 0.16s ease;
      padding: 6px 4px;
      border-radius: 6px;
      overflow: visible;
      font-family: "STKaiti", "KaiTi", "Noto Serif SC", serif;
      font-style: italic;

      &::after {
        /* 毛笔下划（仿手写笔触渐变） */
        content: "";
        position: absolute;
        left: 50%;
        bottom: -10px;
        width: 0;
        height: 8px;
        border-radius: 6px;
        background: linear-gradient(
          90deg,
          rgba(0, 0, 0, 0),
          rgba(46, 138, 163, 0.95),
          rgba(243, 166, 184, 0.9),
          rgba(0, 0, 0, 0)
        );
        transform: translateX(-50%) skewX(-8deg);
        opacity: 0.95;
        filter: blur(0.6px) contrast(1.01);
        transition: width 0.34s cubic-bezier(0.2, 0.9, 0.2, 1), left 0.34s,
          opacity 0.22s;
        background-size: 200% 100%;
        animation: brush-wave 6.4s linear infinite;
      }

      &::before {
        /* 小印章 / 朱砂高光（hover 出现） */
        content: "";
        position: absolute;
        right: 14%;
        top: -6px;
        width: 10px;
        height: 10px;
        border-radius: 2px;
        background: radial-gradient(circle, var(--seal-red), transparent 60%);
        opacity: 0;
        transform: translateY(-6px) scale(0.86);
        transition: opacity 0.26s, transform 0.36s;
        box-shadow: 0 4px 10px rgba(180, 60, 60, 0.06);
      }

      &:hover {
        color: var(--accent);
        transform: translateY(-1.6px);
        text-shadow: 0 0 6px rgba(30, 40, 42, 0.02);
      }

      &:hover::after {
        width: 68%;
        left: 50%;
        opacity: 1;
      }

      &:hover::before {
        opacity: 1;
        transform: translateY(0) scale(1);
      }
    }

    .active-link {
      color: var(--ink-core);
      font-weight: 700;

      &::before {
        /* 小笔饰符，表现画师身份 */
        content: "✒︎";
        position: absolute;
        right: -6px;
        top: 50%;
        transform: translateY(-50%) rotate(-6deg);
        font-size: 12px;
        color: var(--seal-red);
        text-shadow: 0 2px 8px rgba(60, 18, 18, 0.04);
        animation: seal-pulse 3.4s ease-in-out infinite;
        opacity: 0.98;
      }

      &::after {
        width: 92%;
        opacity: 1;
        box-shadow: 0 6px 18px rgba(20, 26, 28, 0.03);
      }
    }
  }

  .hamburger {
    display: none;
    flex-direction: column;
    justify-content: space-around;
    width: 28px;
    height: 24px;
    background: none;
    border: none;
    cursor: pointer;
    padding: 0;

    span {
      display: block;
      width: 100%;
      height: 3px;
      background: rgba(27, 29, 32, 0.92);
      border-radius: 2px;
      transition: transform 0.28s ease, opacity 0.28s ease, background 0.28s;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.06);
    }

    span.open:nth-child(1) {
      transform: translateY(10px) rotate(45deg);
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
    }

    span.open:nth-child(2) {
      opacity: 0;
    }

    span.open:nth-child(3) {
      transform: translateY(-10px) rotate(-45deg);
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
    }
  }

  @media (max-width: 768px) {
    padding: 0 20px;

    .title {
      display: none;
    }
    .hamburger {
      display: flex;
    }

    .nav-links {
      position: absolute;
      top: 64px;
      left: 0;
      right: 0;
      flex-direction: column;
      background: linear-gradient(
        180deg,
        rgba(250, 248, 244, 0.98),
        rgba(244, 240, 233, 0.995)
      );
      backdrop-filter: blur(12px);
      gap: 0;
      overflow: hidden;
      max-height: 0;
      transition: max-height 0.34s ease;
      border-top: 1px solid rgba(46, 138, 163, 0.03);

      &.mobile-open {
        max-height: 520px;
      }

      .nav-item {
        padding: 14px 20px;
        border-bottom: 1px solid rgba(0, 0, 0, 0.03);
      }
    }
  }
}

/* 动效关键帧（笔触 / 墨流 / 鹤影漂移 / 印章脉动） */
@keyframes brush-wave {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

@keyframes brush-pulse {
  0% {
    transform: translateY(0);
  }
  45% {
    transform: translateY(-2px) scale(1.01);
    filter: drop-shadow(0 6px 14px rgba(10, 12, 14, 0.03));
  }
  100% {
    transform: translateY(0);
  }
}

@keyframes fadeInDown {
  0% {
    opacity: 0;
    transform: translateY(-8px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes ink-flow {
  0% {
    transform: translateY(0) rotate(-3deg) translateX(0);
    opacity: 0.88;
    filter: blur(6px);
  }
  50% {
    transform: translateY(-6px) rotate(2deg) translateX(-10px);
    opacity: 1;
    filter: blur(4px) saturate(1.03);
  }
  100% {
    transform: translateY(0) rotate(-3deg) translateX(0);
    opacity: 0.88;
  }
}

@keyframes seal-pulse {
  0% {
    transform: translateY(-6%) rotate(-6deg);
    opacity: 0.8;
    filter: drop-shadow(0 2px 6px rgba(120, 40, 40, 0.02));
  }
  50% {
    transform: translateY(6%) rotate(2deg);
    opacity: 1;
    filter: drop-shadow(0 6px 14px rgba(180, 60, 60, 0.04));
  }
  100% {
    transform: translateY(-6%) rotate(-6deg);
    opacity: 0.8;
  }
}
</style>

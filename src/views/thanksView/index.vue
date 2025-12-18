<template>
  <div class="ack-page">
    <div class="ack-container">
      <h1 class="page-title">鸣谢名单</h1>
      <ul class="ack-list">
        <li
          v-for="(item, index) in acknowledgements"
          :key="index"
          class="ack-item"
          :style="{ animationDelay: index * 0.15 + 's' }"
        >
          <span class="nickname">{{ item.nickname }}:</span>

          <span class="suggestion">{{ item.suggestion }}</span>
        </li>
      </ul>
      <div v-if="acknowledgements.length === 0" class="empty">暂无鸣谢内容</div>
    </div>
    <footer class="ack-footer">© 2025 折枝电子设定集 · 感谢所有支持者</footer>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";
const acknowledgements = reactive([
  {
    nickname: "雪_霁__",
    suggestion: "撷折枝，缀云笺，不负丹青赤子心(人物介绍副标题)",
  },
  {
    nickname: "雪_霁__",
    suggestion:
      "折枝作笔，丹心为炱，挥毫泼墨万物生，白鹤振翅，玄鹿长鸣，墨构乡间赤子心(首页副标题)",
  },
  {
    nickname: "樱绽千秋",
    suggestion:
      "《咏折枝·水墨丹青绘江山》(在文本分享页面)",
  },
  
]);
</script>

<style lang="scss" scoped>
// 折枝主题变量（保留你原有名字，值调整为宣纸/折枝配色）
$bg-deep: #fbf7ef; // 宣纸高光
$deep-2: #efe6d0; // 宣纸暗调
$accent-1: #2a9aa6; // 折枝主色（青）
$accent-2: #e56474; // 朱砂点缀（偏暖粉红）
$text-main: #111213; // 墨色（提高对比）
$silver: #d7e9ee;

$card-bg: rgba(255, 255, 250, 0.96); // 纸张卡片底
$card-border: rgba(10, 10, 8, 0.06);
$glass: rgba(42, 154, 166, 0.04);
$shadow-strong: rgba(5, 6, 8, 0.12);
$shadow-accent: rgba(42, 154, 166, 0.06);

// 页面容器
.ack-page {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  padding-top: 64px;
  background: radial-gradient(
      circle at 12% 14%,
      rgba($accent-1, 0.02),
      transparent 6%
    ),
    linear-gradient(135deg, $bg-deep 0%, $deep-2 50%, $bg-deep 100%);
  background-size: 400% 400%;
  animation: bambooGradient 24s ease infinite;
  color: $text-main;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.ack-container {
  flex: 1;
  padding: 2rem;
  max-width: 720px;
  margin: auto;
  position: relative;
  overflow: visible;
  box-sizing: border-box;
}

// 背景位移动画（竹影/流云感，极轻不干扰阅读）
@keyframes bambooGradient {
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

/* 标题：宣纸 + 毛笔渐变感（提高可读性） */
.page-title {
  font-size: 2rem;
  text-align: center;
  margin-bottom: 1.4rem;
  font-weight: 900;
  line-height: 1.05;
  background: linear-gradient(90deg, $accent-1 0%, $accent-2 78%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
  text-shadow: 0 6px 18px rgba(0, 0, 0, 0.06);
}

/* 可选：轻微的宣纸纹理覆盖层（提高质感） */
.ack-container::before {
  content: "";
  position: absolute;
  inset: 0;
  pointer-events: none;
  background-image: repeating-linear-gradient(
      to right,
      rgba(0, 0, 0, 0.006) 0 1px,
      transparent 1px 36px
    ),
    radial-gradient(circle at 18% 20%, rgba($accent-1, 0.02), transparent 8%),
    radial-gradient(circle at 78% 70%, rgba($accent-2, 0.015), transparent 8%);
  mix-blend-mode: multiply;
  opacity: 0.9;
  border-radius: 10px;
}

/* 列表容器 */
.ack-list {
  list-style: none;
  padding: 0;
  margin: 0;

  .ack-item {
    display: flex;
    align-items: flex-start;
    gap: 0.8rem;
    margin-bottom: 0.9rem;
    padding: 0.85rem 1rem;
    background: $card-bg;
    border-radius: 10px;
    border: 1px solid $card-border;
    box-shadow: 0 6px 18px $shadow-strong, inset 0 0 18px $shadow-accent;
    transition: transform 220ms cubic-bezier(0.2, 0.9, 0.2, 1), box-shadow 220ms,
      background 220ms;
    opacity: 0;
    transform: translateY(8px);
    animation: itemIn 520ms ease forwards;
    line-height: 1.5;

    &:hover {
      transform: translateY(-6px);
      box-shadow: 0 18px 46px rgba(6, 6, 8, 0.12),
        inset 0 0 28px rgba($accent-1, 0.02);
      background: linear-gradient(
        180deg,
        rgba(255, 255, 252, 0.98),
        rgba(250, 246, 238, 0.98)
      );
    }

    /* 名称（用笔触感渐变，但保证对比） */
    .nickname {
      font-weight: 800;
      margin-right: 0.6rem;
      font-size: 0.98rem;
      line-height: 1.25;
      background: linear-gradient(90deg, $accent-1 0%, $silver 100%);
      -webkit-background-clip: text;
      background-clip: text;
      -webkit-text-fill-color: transparent;
      color: transparent;
      text-shadow: 0 0 6px rgba($accent-1, 0.04);
      transition: transform 260ms cubic-bezier(0.2, 0.9, 0.2, 1),
        text-shadow 200ms;
      display: inline-block;
      min-width: 84px;
    }

    /* 主要内容：提高对比和行距，易读 */
    .suggestion {
      flex: 1;
      color: rgba($text-main, 0.94);
      font-size: 0.98rem;
      line-height: 1.65;
      word-break: break-word;
      white-space: pre-wrap;
      letter-spacing: 0.02em;
    }

    /* 可选：在每条条目右侧显示小朱砂印章（点缀） */
    .stamp {
      margin-left: 12px;
      flex-shrink: 0;
      width: 28px;
      height: 28px;
      border-radius: 4px;
      background: linear-gradient(
        135deg,
        rgba($accent-2, 0.98),
        rgba($accent-2, 0.82)
      );
      box-shadow: 0 6px 12px rgba($accent-2, 0.12);
      display: grid;
      place-items: center;
      font-size: 12px;
      font-weight: 900;
      color: #fff;
    }
  }
}

/* 进入动效（逐项有微延迟更雅致） */
@keyframes itemIn {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 空状态 */
.empty {
  text-align: center;
  color: rgba($text-main, 0.64);
  margin-top: 2rem;
  font-size: 0.98rem;
}

/* 页脚：宣纸浅纹 + 轻边 */
.ack-footer {
  text-align: center;
  padding: 1rem 0;
  color: rgba($text-main, 0.68);
  font-size: 0.9rem;
  margin-top: 1.6rem;
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.6),
    rgba(250, 246, 238, 0.6)
  );
  border-top: 1px solid rgba($accent-1, 0.02);
  border-radius: 8px;
}

/* 轻风动效（可按需添加 .breeze 到容器或条目） */
@keyframes breezeLift {
  0% {
    transform: translateY(0);
    opacity: 0.98;
  }
  50% {
    transform: translateY(-6px);
    opacity: 1;
  }
  100% {
    transform: translateY(0);
    opacity: 0.98;
  }
}
.breeze {
  animation: breezeLift 8s ease-in-out infinite;
}

/* 响应式调整 */
@media (max-width: 520px) {
  .ack-container {
    padding: 1.25rem;
    max-width: 92%;
  }
  .page-title {
    font-size: 1.6rem;
  }
  .ack-item {
    padding: 0.7rem 0.8rem;
    border-radius: 8px;
  }
  .nickname {
    font-size: 0.95rem;
    min-width: 68px;
  }
  .suggestion {
    font-size: 0.95rem;
    line-height: 1.6;
  }
  .ack-list .ack-item .stamp {
    display: none;
  }
}
</style>

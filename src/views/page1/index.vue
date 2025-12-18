<template>
  <main class="home">
    <canvas ref="canvasEl" class="rose-canvas" aria-hidden="true"></canvas>

    <!-- 背景轮播（两组用于桌面/移动不同裁切） -->
    <div class="carousel carousel1" aria-hidden="true">
      <img
        v-for="(src, idx) in randomFive"
        :key="idx"
        :src="src"
        class="carousel-image"
        :class="{ active: idx === currentIndex }"
      />
    </div>
    <div class="carousel carousel2" aria-hidden="true">
      <img
        v-for="(src, idx) in randomFive2"
        :key="idx"
        :src="src"
        class="carousel-image"
        :class="{ active: idx === currentIndex }"
      />
    </div>

    <section class="center" role="main">
      <h1 class="title">凝墨绘世，栖心此间· 折枝</h1>

      <div class="subtitle" aria-live="polite">
        <span class="typed">{{ typed }}</span
        ><span class="cursor" aria-hidden="true"></span>
      </div>
    </section>

    <footer
      class="shore-footer-simple"
      role="contentinfo"
      aria-label="页面页脚"
    >
      <div class="inner container">
        <div class="center">
          <div class="slogan">画有尽，意无涯</div>
          <div class="meta">
            © <span>{{ year }}</span> 折枝电子设定集 · 制作：霜落天亦
          </div>
        </div>
      </div>
    </footer>
  </main>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from "vue";
import violet from "@/assets/violet.png"; // 若希望更贴合风格，可替换为“贝壳/羽毛/萤光点”贴图
const year = new Date().getFullYear();
const canvasEl = ref<HTMLCanvasElement | null>(null);
let ctx: CanvasRenderingContext2D;
let animationId = 0;
let lastTime = 0;
let elapsed = 0;

interface Rose {
  baseX: number;
  y: number;
  size: number;
  speed: number;
  swayAmp: number;
  swayFreq: number;
  phase: number;
  angle: number;
  angularSpeed: number;
}

const roses: Rose[] = [];
const ROSE_COUNT_DESKTOP = 18;
const ROSE_COUNT_MOBILE = 6;
const ROSE_IMG = new Image();
ROSE_IMG.src = violet;

function initRoses(count: number) {
  roses.length = 0;
  const canvas = canvasEl.value!;
  const w = canvas.width / (window.devicePixelRatio || 1);
  const h = canvas.height / (window.devicePixelRatio || 1);

  for (let i = 0; i < count; i++) {
    const baseX = Math.random() * w;
    roses.push({
      baseX,
      y: Math.random() * -h,
      size: 48 + Math.random() * 28, // 稍微精简尺寸
      speed: 12 + Math.random() * 36, // 速度更缓
      swayAmp: 12 + Math.random() * 26,
      swayFreq: 0.15 + Math.random() * 0.7,
      phase: Math.random() * Math.PI * 2,
      angle: Math.random() * Math.PI * 2,
      angularSpeed: (Math.random() - 0.5) * 1.2,
    });
  }
  elapsed = 0;
}

let resizeTimeout: number;
function resizeCanvas() {
  window.clearTimeout(resizeTimeout);
  resizeTimeout = window.setTimeout(() => {
    cancelAnimationFrame(animationId);
    const canvas = canvasEl.value!;
    const parent = canvas.parentElement!;
    const dpr = window.devicePixelRatio || 1;
    const w = parent.clientWidth;
    const h = Math.max(parent.clientHeight, 420); // 给个最小高度，避免太窄时粒子不明显

    canvas.style.width = w + "px";
    canvas.style.height = h + "px";
    canvas.width = w * dpr;
    canvas.height = h * dpr;

    ctx.setTransform(1, 0, 0, 1, 0, 0);
    ctx.scale(dpr, dpr);

    const isMobile = w < 768;
    initRoses(isMobile ? ROSE_COUNT_MOBILE : ROSE_COUNT_DESKTOP);
    lastTime = 0;
    animationId = requestAnimationFrame(tickCanvas);
  }, 160);
}

function tickCanvas(now: number) {
  if (!lastTime) lastTime = now;
  const dt = (now - lastTime) / 1000;
  lastTime = now;
  elapsed += dt;

  const canvas = canvasEl.value!;
  const w = canvas.clientWidth;
  const h = canvas.clientHeight;

  ctx.clearRect(0, 0, w, h);

  // 轻微整体雾层，增强深度（透明度低，避免影响可读性）
  ctx.fillStyle = "rgba(2,8,14,0.08)";
  ctx.fillRect(0, 0, w, h);

  roses.forEach((r) => {
    r.y += r.speed * dt;
    const sway = r.swayAmp * Math.sin(r.phase + elapsed * r.swayFreq);
    const x = r.baseX + sway;
    r.angle += r.angularSpeed * dt;

    if (r.y > h + r.size) {
      r.y = -r.size * 0.6;
      r.baseX = Math.random() * w;
      r.phase = Math.random() * Math.PI * 2;
    }

    if (x > w + r.size || x < -r.size) return;

    // 计算透明度：越远看上去越淡
    const alpha = Math.max(0, Math.min(1, 1 - (r.y / h) * 0.6));

    ctx.save();
    ctx.globalAlpha = alpha;
    ctx.translate(x, r.y);
    ctx.rotate(r.angle);

    if (ROSE_IMG && ROSE_IMG.complete && ROSE_IMG.naturalWidth > 0) {
      // 使用图片绘制，但加上一层冷色调叠加（globalCompositeOperation 简单处理）
      ctx.drawImage(ROSE_IMG, -r.size / 2, -r.size / 2, r.size, r.size);

      // 轻微冷光叠加，提升风格一致性
      ctx.globalCompositeOperation = "lighter";
      const grad = ctx.createRadialGradient(0, 0, 0, 0, 0, r.size);
      grad.addColorStop(0, `rgba(79,233,223,${0.08 * alpha})`);
      grad.addColorStop(1, "rgba(0,0,0,0)");
      ctx.fillStyle = grad;
      ctx.fillRect(-r.size / 2, -r.size / 2, r.size, r.size);
      ctx.globalCompositeOperation = "source-over";
    }

    ctx.restore();
  });

  animationId = requestAnimationFrame(tickCanvas);
}

// ========== 打字机文案 ==========
// 适合折枝风格的副标题（偏长句，已为打字器准备）
const lines = [
  "折枝作笔，丹心为炱，挥毫泼墨万物生",
  "白鹤振翅，玄鹿长鸣，墨构乡间赤子心",
  "在这里，我只想安静地分享我的画，和画背后的心事。",
  "笔尖流淌的，不只是色彩，还有我走过的路和遇见的光。",
  "不善言辞，所以我把所有的言语，都藏在了笔墨的浓淡里。",
  "这里是我漂泊途中，用心布置的一处小小“画乡”。",
  "每一幅画，都是一段沉默的独白，期待与你温柔共鸣。",
  "以画笔为舟，以色彩为语，邀你共游我心中的静谧之境。",
  "从瑝珑到今州，从展示品到流浪者… 画笔始终是我唯一的语言。",
  "不必追问过去，如果你愿意，可以从任何一幅画开始认识我。",
  "这里收集了一个不善交际的画师，所能给出的全部真诚。",
  "感谢你驻足于此，聆听这些由线条与色彩编织的“声音”。",
];

const typed = ref("");
let lineIndex = 0;
let charIndex = 0;
let deleting = false;
let timer: number | null = null;

const TYPING = 120;
const DELETING = 40;
const PAUSE = 1200;

function tick() {
  const cur = lines[lineIndex];
  if (!deleting) {
    typed.value = cur.slice(0, charIndex + 1);
    charIndex++;
    if (charIndex >= cur.length) {
      timer = window.setTimeout(() => {
        deleting = true;
        tick();
      }, PAUSE);
      return;
    }
    timer = window.setTimeout(tick, TYPING);
  } else {
    typed.value = cur.slice(0, charIndex - 1);
    charIndex--;
    if (charIndex <= 0) {
      deleting = false;
      lineIndex = (lineIndex + 1) % lines.length;
      timer = window.setTimeout(tick, 360);
      return;
    }
    timer = window.setTimeout(tick, DELETING);
  }
}

// ========== 背景图片导入与轮播 ==========
const modules = import.meta.glob("@/assets/images1/*.{jpg,png,jpeg,webp}", {
  eager: true,
});
const allSrcs: string[] = Object.values(modules).map((mod: any) => mod.default);

const modules2 = import.meta.glob("@/assets/images2/*.{jpg,png,jpeg,webp}", {
  eager: true,
});
const allSrcs2: string[] = Object.values(modules2).map(
  (mod: any) => mod.default
);

function shuffle<T>(arr: T[]): T[] {
  const a = arr.slice();
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}
const randomFive = ref<string[]>(shuffle(allSrcs).slice(0, 5));
const randomFive2 = ref<string[]>(shuffle(allSrcs2).slice(0, 5));

const currentIndex = ref(0);
let Imgtimer: number | undefined;

onMounted(() => {
  timer = window.setTimeout(tick, 420);

  Imgtimer = window.setInterval(() => {
    currentIndex.value =
      (currentIndex.value + 1) % Math.max(1, randomFive.value.length);
  }, 5200);

  const canvas = canvasEl.value!;
  ctx = canvas.getContext("2d")!;

  // 当图片加载或资源就绪后调整 canvas 大小并启动渲染
  ROSE_IMG.onload = () => {
    resizeCanvas();
  };
  // 如果图片已经加载完（缓存情况），也要触发 init
  if (ROSE_IMG.complete && ROSE_IMG.naturalWidth > 0) {
    resizeCanvas();
  }

  window.addEventListener("resize", resizeCanvas);
});

onBeforeUnmount(() => {
  if (Imgtimer) clearInterval(Imgtimer);
  if (timer) window.clearTimeout(timer);

  cancelAnimationFrame(animationId);
  window.removeEventListener("resize", resizeCanvas);
});
</script>

<style lang="scss" scoped>
/* 折枝风格 - 宣纸 / 毛笔墨迹 / 朱砂点缀 */
$paper-start: #fbf7f0; // 宣纸高光
$paper-end: #f1e8da; // 宣纸暗调
$ink-core: #0b0d0f; // 墨核深色
$ink-wash: rgba(11, 13, 15, 0.06); // 墨洗薄层
$accent: #2e8aa3; // 折枝服饰青色系（主色）
$accent-2: #f3a6b8; // 次色 / 粉点（高光）
$seal-red: #b94b4b; // 朱砂印章色
$text-main: #162022; // 文字主色
$paper-fiber: rgba(10, 8, 6, 0.02); // 纸纤维纹理色
@use "sass:color";
.home {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
  overflow: hidden;
  color: $text-main;
  font-family: Inter, "PingFang SC", "Noto Sans CJK SC", "Microsoft YaHei",
    sans-serif;
  background:
    /* 轻微墨晕与笔痕层（放在最上） */ radial-gradient(
      circle at 12% 14%,
      rgba($accent, 0.02),
      transparent 8%
    ),
    radial-gradient(circle at 84% 76%, rgba($accent-2, 0.02), transparent 8%),
    /* 纸张底色 */ linear-gradient(180deg, $paper-start 0%, $paper-end 100%);

  /* 纸纤维/细纹层（可增强真实纸感） */
  &::before {
    content: "";
    position: absolute;
    inset: -6% -8%;
    z-index: 1;
    background-image: repeating-linear-gradient(
      to right,
      $paper-fiber 0px,
      rgba(255, 255, 255, 0) 1px,
      rgba(255, 255, 255, 0) 28px
    );
    opacity: 0.55;
    mix-blend-mode: multiply;
    pointer-events: none;
    filter: contrast(0.98) saturate(0.95);
    transform: translateY(-2%);
  }

  /* 背景画布：墨渍 / 鹤影占位（可插入实际 SVG .crane-shadow） */
  .rose-canvas {
    position: absolute;
    inset: 0;
    z-index: 2;
    pointer-events: none;

    /* 预留：若使用 .crane-shadow SVG 放在 DOM 中, 它应该具有 z-index:2 并使用 mix-blend-mode:multiply */
  }

  /* 轮播区（图片淡入 + 纸张/笔触滤镜）*/
  .carousel {
    position: absolute;
    inset: 0;
    z-index: 0;
    pointer-events: none;

    &::before {
      content: "";
      position: absolute;
      inset: 0;
      /* 轻微竖向纸纹（模仿宣纸纹理） */
      background-image: repeating-linear-gradient(
        to bottom,
        rgba(0, 0, 0, 0.01) 0px,
        rgba(0, 0, 0, 0.01) 1px,
        transparent 1px,
        transparent 16px
      );
      opacity: 0.06;
      mix-blend-mode: overlay;
      z-index: 2;
      transform: translateY(-3%);
      animation: scroll-wave 18s linear infinite;
      pointer-events: none;
    }

    .carousel-image {
      position: absolute;
      width: 100%;
      height: 100%;
      object-fit: cover;
      opacity: 0;
      transition: opacity 700ms ease, transform 10s linear;
      filter: blur(0.6px) saturate(0.85) contrast(0.95) brightness(0.96);
      transform: scale(1.02);
      mix-blend-mode: multiply;

      &.active {
        opacity: 1;
        transform: scale(1);
      }
    }
  }

  .carousel2 {
    display: none;
  }

  /* 确保文件顶部有：@use "sass:color"; */

  .center {
    position: relative;
    z-index: 4;
    flex: 1 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 40px 24px;
    gap: 10px;

    /* 主标题：增强版（更大、更立体、更易读） */
    .title {
      font-size: 5rem; /* 放大视觉体量 */
      font-weight: 900;
      margin: 0;
      line-height: 1;
      background: linear-gradient(90deg, $accent 0%, $accent-2 86%);
      -webkit-background-clip: text;
      background-clip: text;
      -webkit-text-fill-color: transparent;
      color: $text-main;
      letter-spacing: 0.2px;
      position: relative;
      display: inline-block;
      padding: 10px 12px;
      transform-origin: center;
      transition: transform 0.28s ease, text-shadow 0.28s ease;

      /* 更明显的“毛笔重墨”描边（细微描边让浅背景上更清晰） */
      -webkit-text-stroke: 0.6px rgba(11, 13, 15, 0.18);

      &:hover {
        transform: translateY(-3px) scale(1.01);
      }
    }

    /* 副标题：提高对比度与尺寸，同时保留书法风格 */
    .subtitle {
      font-size: 2.4rem; /* 提升字号以增加可读性 */
      min-height: 1.6em;
      /* 使用 color.adjust 使颜色更深、更容易阅读 */
      color: color.adjust($text-main, $lightness: -8%);
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      font-family: "Dancing Script", "STKaiti", "Segoe Script", cursive;
      font-weight: 500;
      letter-spacing: 0.06em;
      position: relative;
      padding: 8px 14px;

      /* 半透明纸带底，增加对比不破坏纸张质感 */
      &::before {
        content: "";
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        width: calc(100% + 36px);
        height: calc(100% + 6px);
        border-radius: 6px;
        background: linear-gradient(
          90deg,
          rgba(255, 255, 255, 0.6),
          rgba(250, 248, 244, 0.88)
        );
        box-shadow: 0 6px 18px rgba(6, 8, 10, 0.06) inset;
        z-index: -1;
        pointer-events: none;
        opacity: 0.92;
        mix-blend-mode: normal;
        filter: blur(0.6px);
      }

      .typed {
        display: inline-block;
        font-weight: 700;
        color: color.adjust($text-main, $lightness: -12%); /* 更深一点 */
        letter-spacing: 0.08em;
      }

      .cursor {
        width: 12px;
        height: 1.05em;
        margin-left: 6px;
        background: linear-gradient(180deg, $seal-red, $accent-2);
        border-radius: 2px;
        animation: blink 1s steps(1) infinite;
        transform: translateY(2px);
        filter: drop-shadow(0 6px 14px rgba($seal-red, 0.06));
      }
    }
  }

  /* 页脚：宣纸收口 + 朱砂细线 */
  .shore-footer-simple {
    background: linear-gradient(
      180deg,
      rgba(245, 240, 232, 0.76),
      rgba(242, 235, 225, 0.68)
    );
    border-top: 1px solid rgba($seal-red, 0.06);
    color: $text-main;
    font-size: 13px;
    position: relative;
    overflow: visible;
    z-index: 3;

    .inner.container {
      width: min(1100px, 94%);
      margin: 0 auto;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
    }

    .center {
      text-align: center;
      flex: 1 1 auto;

      .slogan {
        background: linear-gradient(90deg, $accent 0%, $accent-2 60%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        display: inline-block;
        line-height: 1;
        font-size: 14px;
        letter-spacing: 0.3px;
        text-shadow: 0 6px 20px rgba(6, 4, 8, 0.06);
      }

      .meta {
        color: rgba($text-main, 0.66);
        margin-top: 6px;
        font-size: 12px;
      }
    }
  }
}

/* 单独浮动印章/纸片（可插入 .floating-note 在 DOM）*/
.floating-note {
  position: absolute;
  font-size: 14px;
  color: $seal-red;
  opacity: 0.95;
  transform-origin: center;
  animation: seal-float 5.6s ease-in-out infinite;
  filter: drop-shadow(0 6px 18px rgba($seal-red, 0.06));
}

/* === 动画关键帧（墨流 / 书卷摇曳 / 飞墨 / 印章漂浮） === */
@keyframes scroll-wave {
  0% {
    transform: translateY(-6%);
    opacity: 0.92;
  }
  50% {
    transform: translateY(6%);
    opacity: 0.98;
  }
  100% {
    transform: translateY(-6%);
    opacity: 0.92;
  }
}

@keyframes brush-sway {
  0% {
    transform: translateY(0) rotate(-0.8deg);
  }
  50% {
    transform: translateY(-6px) rotate(0.8deg);
  }
  100% {
    transform: translateY(0) rotate(-0.8deg);
  }
}

@keyframes ink-drift {
  0% {
    transform: translateX(0) translateY(0);
    opacity: 0.92;
  }
  50% {
    transform: translateX(-10px) translateY(-6px);
    opacity: 1;
  }
  100% {
    transform: translateX(0) translateY(0);
    opacity: 0.92;
  }
}

@keyframes seal-float {
  0% {
    transform: translateY(0) rotate(-3deg) scale(0.98);
    opacity: 0.86;
  }
  50% {
    transform: translateY(-8px) rotate(2deg) scale(1.02);
    opacity: 1;
  }
  100% {
    transform: translateY(0) rotate(-3deg) scale(0.98);
    opacity: 0.86;
  }
}

@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

/* 响应式：移动优先 */
@media (max-width: 720px) {
  .home {
    .carousel {
      display: none;
    }
    .carousel2 {
      display: block;
    }

    .center {
      padding: 18px 14px;

      .title {
        font-size: 2.2rem;
      }

      .subtitle {
        font-size: 1.4rem;
      }
    }
  }
}
</style>

<template>
  <div class="yuzuki-resources">
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
    <header class="hero">
      <div class="hero-inner">
        <h1>资源分享</h1>
        <p class="subtitle">可自由上传关于折枝的相关链接</p>
      </div>
    </header>

    <main class="container">
      <section class="uploader" :class="{ collapsed: uploaderCollapsed }">
        <div class="uploader-head">
          <button
            class="toggle"
            @click="toggleUploader"
            :aria-expanded="!uploaderCollapsed"
          >
            <span v-if="uploaderCollapsed">展开上传区</span>
            <span v-else>收起上传区</span>
          </button>
        </div>

        <form
          @submit.prevent="addResource"
          class="upload-form"
          :aria-hidden="uploaderCollapsed"
        >
          <div class="row">
            <input
              v-model="form.title"
              type="text"
              placeholder="标题（必填，如果有解压码之类的也写这里吧）"
              aria-label="标题"
            />
            <input
              v-model="form.type"
              type="text"
              placeholder="链接类型(网页链接、b站视频、网盘链接等等)"
              aria-label="来源"
            />
          </div>

          <div class="row">
            <input
              v-model="form.uploader"
              type="text"
              placeholder="上传人（可选）"
              aria-label="上传人"
            />
            <input
              v-model="form.link"
              type="url"
              placeholder="链接(只输入网址不能有中文)"
              aria-label="链接"
            />
          </div>

          <div class="actions">
            <button type="submit" class="btn primary">上传</button>
          </div>
        </form>
      </section>

      <section class="list">
        <div class="list-header">
          <h2>资源列表（{{ resources.length }}）</h2>
          <div class="sort">
            <label>
              排序：
              <select v-model="sortBy">
                <option value="time">按时间（新→旧）</option>
                <option value="likes">按点赞（高→低）</option>
              </select>
            </label>
          </div>
        </div>

        <ul class="items">
          <li v-for="item in sortedResources" :key="item.id" class="item">
            <a
              :href="item.link"
              target="_blank"
              rel="noopener noreferrer"
              class="title"
              >{{ item.title }}</a
            >

            <div class="meta">
              <div class="left">
                <span class="uploader">{{ item.uploader || "匿名" }}</span>
                <span class="dot">•</span>
                <time :datetime="item.time">{{ formatTime(item.time) }}</time>
              </div>

              <div class="right">
                <button
                  @click.prevent="handleLike(item)"
                  :aria-pressed="likedIds.has(String(item.id))"
                  class="like-btn"
                  :class="{ active: likedIds.has(String(item.id)) }"
                >
                  <img
                    :src="
                      likedIds.has(String(item.id))
                        ? '/icons/heart-red-filled.svg'
                        : '/icons/heart-red-outline.svg'
                    "
                    class="heart-icon"
                    alt="heart"
                  />
                  <span class="count">{{ item.likes }}</span>
                </button>

                <span class="badge">{{ item.type }}</span>
              </div>
            </div>
          </li>
        </ul>

        <p v-if="resources.length === 0" class="empty">
          目前没有资源，快来上传第一条吧！
        </p>
      </section>
    </main>

    <footer class="foot">提示：点击标题将直接跳转</footer>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onBeforeUnmount } from "vue";
// 如果你的工程使用 ts 路径别名 @ 指向 src，可以用 '@/api/resource'，否则根据实际路径调整
import {
  getResourceList,
  createResource,
  likeResource,
} from "@/api/modules/resource";
import { ElMessage } from "element-plus";

interface Resource {
  id: number | string;
  title: string;
  uploader?: string;
  time: string; // ISO 或 created_at
  likes: number;
  link: string;
  type: string;
  role_key?: string;
}

const STORAGE_KEY = "fll_resources_v1";
const DEFAULT_ROLE = "zhezhi";

const form = ref<{
  title: string;
  uploader: string;
  link: string;
  type: string;
}>({
  title: "",
  uploader: "",
  link: "",
  type: "",
});

const resources = ref<Resource[]>([]);
const likedIds = ref(new Set<string>());
const sortBy = ref<"time" | "likes">("time");
const uploaderCollapsed = ref(false);

function mapServerToLocal(row: any): Resource {
  return {
    id: row.id,
    title: row.title,
    uploader: row.uploader || "匿名",
    time: row.created_at || row.time || new Date().toISOString(),
    likes: row.likes ?? 0,
    link: row.link,
    type: row.storage_type || row.type || "other",
    role_key: row.role_key,
  };
}

async function loadResources() {
  try {
    // 尝试从后端拉取（分页可扩展，这里一次拉足够 demo）
    const res: any = await getResourceList({
      role_key: DEFAULT_ROLE,
      page: 1,
      pageSize: 100,
    });
    if (res && res.success && Array.isArray(res.data)) {
      resources.value = res.data.map(mapServerToLocal);
      // 可恢复本地点赞状态（仅 UI 记忆）
      const raw = localStorage.getItem(STORAGE_KEY);
      if (raw) {
        try {
          const parsed = JSON.parse(raw);
          if (parsed.liked && Array.isArray(parsed.liked)) {
            parsed.liked.forEach((id: string) => likedIds.value.add(id));
          }
        } catch (e) {
          /* ignore */
        }
      }
      return;
    }
  } catch (err) {
    console.warn("拉取资源失败，使用本地缓存", err);
  }
  // 回退：本地缓存（仅恢复点赞状态）
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (raw) {
      const parsed = JSON.parse(raw);
      if (parsed.liked && Array.isArray(parsed.liked)) {
        parsed.liked.forEach((id: string) => likedIds.value.add(id));
      }
    }
  } catch (e) {
    console.warn("本地加载失败", e);
  }
}

function saveLocalCache() {
  try {
    const liked = Array.from(likedIds.value);
    localStorage.setItem(STORAGE_KEY, JSON.stringify({ liked }));
  } catch (e) {
    console.warn("保存本地缓存失败", e);
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
  loadResources();
  Imgtimer = window.setInterval(() => {
    currentIndex.value =
      (currentIndex.value + 1) % Math.max(1, randomFive.value.length);
  }, 5200);
  uploaderCollapsed.value = window.innerWidth <= 640;
});
function toggleUploader() {
  uploaderCollapsed.value = !uploaderCollapsed.value;
}
onBeforeUnmount(() => {
  if (Imgtimer) clearInterval(Imgtimer);
});

async function addResource() {
  const t = form.value.title.trim();
  const l = form.value.link.trim();
  if (!form.value.title.trim() || !form.value.link.trim()) {
    return ElMessage.warning("请填写完整信息");
  }
  if (!/^https?:\/\//i.test(l)) {
    return ElMessage.error("请输入正确的链接(https开头)");
  }
  // 尝试调用后端接口
  try {
    const payload = {
      title: t,
      uploader: form.value.uploader.trim() || "匿名",
      link: l,
      storage_type: form.value.type,
      role_key: DEFAULT_ROLE,
    };
    const res: any = await createResource(payload);
    if (res && res.success && res.data) {
      const added = mapServerToLocal(res.data);
      resources.value.unshift(added);
      // 自动展开到顶部展示（可选）
      saveLocalCache();
      resetForm();
      ElMessage.success("上传成功");
      return;
    }
    ElMessage.error("上传失败");
  } catch (err) {
    console.warn("创建资源失败", err);
  }
}

function resetForm() {
  form.value.title = "";
  form.value.uploader = "";
  form.value.link = "";
  form.value.type = "";
}

async function handleLike(item: Resource) {
  // UI 乐观更新
  const id = item.id;
  const wasLiked = likedIds.value.has(String(id));
  if (wasLiked) {
    likedIds.value.delete(String(id));
    item.likes = Math.max(0, item.likes - 1);
  } else {
    likedIds.value.add(String(id));
    item.likes++;
  }
  saveLocalCache();

  // 同步后端（不依赖返回值进行 UI 回滚，简单处理：若失败则回退）
  try {
    const action = wasLiked ? "unlike" : "like";
    const res: any = await likeResource(id, action);
    if (
      res &&
      res.success &&
      res.data &&
      typeof res.data.likes !== "undefined"
    ) {
      item.likes = res.data.likes;
    }
  } catch (err) {
    console.warn("点赞接口调用失败，回滚本地状态", err);
    // 回滚
    if (wasLiked) {
      // 本来是已赞，取消失败 -> 重新添加
      likedIds.value.add(String(id));
      item.likes++;
    } else {
      likedIds.value.delete(String(id));
      item.likes = Math.max(0, item.likes - 1);
    }
    saveLocalCache();
  }
}

const sortedResources = computed(() => {
  const arr = [...resources.value];
  if (sortBy.value === "time") {
    arr.sort((a, b) => +new Date(b.time) - +new Date(a.time));
  } else {
    arr.sort((a, b) => b.likes - a.likes);
  }
  return arr;
});

function formatTime(iso: string) {
  try {
    const d = new Date(iso);
    return new Intl.DateTimeFormat("zh-CN", {
      month: "2-digit",
      day: "2-digit",
      hour: "2-digit",
      minute: "2-digit",
    }).format(d);
  } catch (e) {
    return iso;
  }
}
</script>

<style scoped lang="scss">
@use "sass:color";

/* ---------------- 折枝色板（可调整） ---------------- */
$paper-1: #fbf6ef; // 宣纸高光
$paper-2: #efe6d0; // 宣纸暗调
$ink: #121416; // 墨色文字（深且柔和）
$muted: rgba($ink, 0.72); // 次要文字

$branch: #2b9aa6; // 折枝主色（青）
$seed: #ef7b88; // 朱砂点缀（暖）
$card-border: rgba(43, 154, 166, 0.06);
$soft-shadow: rgba(8, 10, 12, 0.46);
$glass: rgba(255, 255, 255, 0.02);

/* 全局基底 */
.yuzuki-resources {
  color: $ink;
  display: flex;
  flex-direction: column;
  padding-top: 64px;
  font-family: "Noto Sans SC", "PingFang SC", "Inter", system-ui, sans-serif;
  -webkit-font-smoothing: antialiased;
  background: linear-gradient(180deg, $paper-2 0%, $paper-1 100%);
  min-height: 100vh;
  position: relative;
  overflow: hidden;
}

/* 画布装饰位置（放鹤影或枝影）*/
.rose-canvas {
  position: absolute;
  inset: 0;
  z-index: 1;
  pointer-events: none;

  .crane-shadow {
    position: absolute;
    right: 6%;
    top: 6%;
    width: 420px;
    height: 120px;
    opacity: 0.06;
    filter: blur(6px);
    transform: rotate(-4deg);
    mix-blend-mode: multiply;
    pointer-events: none;
  }

  @media (max-width: 720px) {
    .crane-shadow {
      display: none;
    }
  }
}

/* carousel（背景层） */
.carousel {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;

  &::before {
    content: "";
    position: absolute;
    inset: 0;
    background: linear-gradient(
      180deg,
      rgba(6, 6, 6, 0.06),
      rgba(10, 8, 10, 0.08)
    );
    mix-blend-mode: soft-light;
    pointer-events: none;
  }

  .carousel-image {
    position: absolute;
    width: 100%;
    height: 100%;
    object-fit: cover;
    filter: blur(0.8px) saturate(0.9) brightness(0.96);
    opacity: 0;
    transition: opacity 0.9s ease, transform 12s linear;

    &.active {
      opacity: 1;
      transform: scale(1);
    }
  }
}
.carousel2 {
  display: none;
}
/* hero（顶部） */
.hero {
  padding: 18px 16px;
  background: linear-gradient(
    180deg,
    rgba(250, 245, 238, 0.8),
    rgba(242, 236, 224, 0.86)
  );
  border-bottom: 1px solid $card-border;
  z-index: 6;
  position: relative;

  .hero-inner {
    max-width: 1100px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 6px;
  }

  h1 {
    margin: 0;
    font-size: 20px;
    font-weight: 900;
    color: $branch;
    letter-spacing: 0.5px;
    text-shadow: 0 6px 18px rgba(0, 0, 0, 0.06);
    display: inline-block;
  }

  .subtitle {
    margin-top: 6px;
    color: rgba($ink, 0.66);
    font-size: 13px;
  }
}

/* container */
.container {
  max-width: 1100px;
  margin: 18px auto;
  padding: 0 12px;
  width: 100%;
  box-sizing: border-box;
  z-index: 8;
  position: relative;
}

/* uploader 卡片（折枝样式） */
.uploader {
  border-radius: 14px;
  padding: 0;
  box-shadow: 0 18px 56px $soft-shadow;
  border: 1px solid rgba($seed, 0.04);
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.02),
    rgba(245, 240, 232, 0.02)
  );
  overflow: hidden;

  .uploader-head {
    display: flex;
    justify-content: flex-end;
    padding: 10px 12px;
    background: linear-gradient(
      180deg,
      rgba(250, 248, 244, 0.22),
      rgba(242, 236, 226, 0.16)
    );
  }

  .toggle {
    background: transparent;
    border: 1px solid rgba($branch, 0.12);
    color: $branch;
    padding: 6px 10px;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 700;
    box-shadow: inset 0 -2px 6px rgba(0, 0, 0, 0.06);
    transition: transform 0.14s ease, box-shadow 0.16s ease, background 0.18s;
  }

  .upload-form {
    padding: 16px;
    transition: max-height 0.28s ease, padding 0.28s ease;
    max-height: 1200px;

    .row {
      display: flex;
      gap: 10px;
      margin-bottom: 12px;
      align-items: center;

      input,
      select,
      textarea {
        flex: 1 1 0;
        padding: 10px 12px;
        border-radius: 10px;
        border: 1px solid rgba($branch, 0.08);
        font-size: 14px;
        background: linear-gradient(
          180deg,
          rgba(255, 255, 255, 0.98),
          rgba(252, 250, 247, 0.98)
        ); /* 轻纸色输入，提高文字可读 */
        color: $ink;
        font-weight: 600;
        outline: none;
        transition: box-shadow 0.16s, border-color 0.16s, transform 0.08s;
        box-shadow: inset 0 -3px 8px rgba(0, 0, 0, 0.06);
      }

      select {
        max-width: 160px;
      }

      input::placeholder,
      textarea::placeholder {
        color: rgba($ink, 0.42);
      }

      input:focus,
      select:focus,
      textarea:focus {
        border-color: $branch;
        box-shadow: 0 10px 30px rgba($branch, 0.06);
        transform: translateY(-1px);
      }
    }

    .actions {
      display: flex;
      gap: 10px;
      align-items: center;
      margin-top: 6px;

      .btn {
        padding: 8px 12px;
        border-radius: 10px;
        border: none;
        font-weight: 700;
        cursor: pointer;
        transition: transform 0.12s ease, box-shadow 0.14s ease;
      }

      .btn.primary {
        background: linear-gradient(135deg, $branch 0%, $seed 56%);
        color: #121212;
        box-shadow: 0 10px 30px rgba(40, 30, 30, 0.06);
      }

      .btn.secondary {
        background: transparent;
        color: rgba(100, 80, 90, 0.95);
        border: 1px solid rgba($seed, 0.06);
      }

      .btn:active {
        transform: translateY(1px) scale(0.998);
      }
    }
  }

  &.collapsed {
    .upload-form {
      max-height: 0;
      padding: 0;
      overflow: hidden;
    }
  }
}

/* 列表（文件/资源） */
.list {
  margin-top: 18px;

  .list-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
    h2 {
      font-size: 16px;
      margin: 0;
      color: $branch;
      font-weight: 800;
    }
    .sort select {
      padding: 8px;
      border-radius: 8px;
      border: 1px solid rgba($seed, 0.08);
      background: linear-gradient(
        180deg,
        rgba(250, 248, 246, 0.98),
        rgba(246, 242, 238, 0.98)
      );
      color: $branch;
    }
  }

  .items {
    list-style: none;
    padding: 0;
    margin: 0;
    max-height: 60vh;
    overflow: auto;
    padding-right: 6px;
  }

  .item {
    border-radius: 12px;
    padding: 12px;
    margin-bottom: 12px;
    background: linear-gradient(
      180deg,
      rgba(255, 255, 255, 0.98),
      rgba(250, 246, 240, 0.98)
    ); /* 纸色卡片 */
    border: 1px solid rgba(20, 20, 20, 0.03);
    transition: transform 0.18s cubic-bezier(0.2, 0.9, 0.25, 1),
      box-shadow 0.18s ease;
    box-shadow: 0 8px 28px rgba(10, 12, 10, 0.04);

    &:hover {
      transform: translateY(-6px);
      box-shadow: 0 18px 48px rgba(8, 10, 12, 0.06);
    }

    .title {
      display: block;
      color: $ink;
      font-weight: 800;
      margin-bottom: 8px;
      font-size: 15px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }

    .meta {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 13px;
      color: $muted;

      .left {
        display: flex;
        align-items: center;
        gap: 8px;
        .uploader {
          color: $seed;
          font-weight: 700;
        }
        .dot {
          opacity: 0.6;
        }
        time {
          color: rgba($ink, 0.7);
          font-weight: 600;
        }
      }

      .right {
        display: flex;
        align-items: center;
        gap: 8px;
        .like-btn {
          background: transparent;
          border: none;
          cursor: pointer;
          padding: 6px 8px;
          border-radius: 8px;
          font-weight: 700;
          display: inline-flex;
          align-items: center;
          gap: 6px;
          color: rgba(90, 80, 86, 0.9);
          transition: transform 0.08s, background 0.12s;
        }
        .like-btn:hover {
          transform: translateY(-2px);
        }
        .heart-icon {
          width: 18px;
          height: 18px;
          display: block;
          filter: grayscale(100%) opacity(0.9);
        }
        .like-btn.active .heart-icon {
          filter: none;
          transform: scale(1.06);
          box-shadow: 0 6px 20px rgba($branch, 0.12);
        }

        .badge {
          padding: 4px 8px;
          border-radius: 999px;
          font-size: 12px;
          font-weight: 700;
          background: linear-gradient(
            180deg,
            rgba($seed, 0.12),
            rgba(160, 110, 120, 0.06)
          );
          color: $branch;
          border: 1px solid rgba($seed, 0.04);
        }
      }
    }
  }

  .empty {
    text-align: center;
    color: $branch;
    padding: 28px 0;
  }
}

/* footer */
.foot {
  text-align: center;
  color: rgba($muted, 0.86);
  font-size: 12px;
  margin: 20px 0 40px;
}

/* responsive */
@media (max-width: 720px) {
  .carousel1 {
    display: none;
  }
  .carousel2 {
    display: block;
  }
  .hero {
    padding: 12px 10px;
    h1 {
      font-size: 18px;
    }
    .subtitle {
      font-size: 12px;
      color: rgba($ink, 0.82);
    }
  }
  .container {
    padding: 0 14px;
  }
  .upload-form .row {
    flex-direction: column;
    input {
      width: 100%;
    }
  }
  .upload-form .actions {
    flex-direction: column;
    align-items: stretch;
  }
  .items .item .title {
    white-space: normal;
  }
  .rose-canvas .crane-shadow {
    display: none;
  }
}
</style>

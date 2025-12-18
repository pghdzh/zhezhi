<template>
  <div class="megumi-page" aria-live="polite">
    <main class="container" ref="mainContainer">
      <!-- HERO 区域 -->
      <header class="hero" role="banner">
        <div class="left-col">
          <button class="avatar-card">
            <picture class="avatar-wrap">
              <img
                src="@/assets/images1/zhezhi-slty (8).webp"
                class="avatar"
                loading="lazy"
                decoding="async"
              />
            </picture>
            <div class="avatar-meta">
              <span class="avatar-badge">共鸣能力：笔落春风</span>
            </div>
          </button>
        </div>
        <div class="right-col" ref="metaCard">
          <div class="hero-panel">
            <h1 class="name">
              折枝
              <span class="jp">（鬻画维生的委托画师）</span>
            </h1>
            <p class="subtitle">墨痕即心迹，丹青即独白</p>
            <p class="lead">
              忧思敏感，不善言辞，所思所想皆寄寓于画，笔下万物可幻化而出，惟妙惟肖。
            </p>
          </div>
        </div>
      </header>

      <!-- 内容卡片区 -->
      <section id="personality" class="card personality">
        <div class="card-head">
          <h2>核心机制</h2>
        </div>
        <ul class="traits">
          <li>
            【灵韵】：血条上方上限90点的特殊能量条，每30点一段，共三段。常态攻击、变奏技能可回复【灵韵】。<br />
            至少拥有30点【灵韵】，施放重击·构形召唤鹤影·中。<br />
            至少拥有60点【灵韵】，施放共鸣技能以形写神召唤鹤影·左和鹤影·右。
          </li>
          <li>
            【鹤影】：共鸣技能以形写神在地面短按时，鹤影会召唤在地面；在地面长按或空中短按，鹤影会召唤在空中。<br />
            当鹤影·左或鹤影·右在空中时，鹤影·中也会召唤在空中。
          </li>
          <li>
            【神来之笔】：存在【鹤影】时，共鸣技能会变为【神来之笔】（飞雷神），每次施放获得一层【极意】，拥有两层【极意】时，共鸣技能会进化为【极意·神来之笔】，施放提高普攻伤害加成。
          </li>
          <li style="font-weight: bold">输出流程：</li>
          <li>基础流程：A-A-A-（可切人）-E-A-E2-E2-R-E3-Q</li>
          <li>
            技能缩写：普攻 = A，重击 = Z，共鸣技能 = E，共鸣解放 = R，声骸 = Q
          </li>
        </ul>
      </section>

      <section id="resonance-status" class="card overview">
        <div class="card-head">
          <h2>共鸣状况</h2>
        </div>
        <div class="two-col">
          <div class="text-block">
            <p>
              对象无明确共鸣时间，幼年时期的画作已有“活化”征兆，后在绘画练习中逐渐提升对能力的掌控。<br />
              对象声痕位于右手，共鸣后体征未发生明显变化。<br />
              经测试，对象可以“活化”自己绘制的图像，并对旁观者视觉造成影响，干扰旁观者判断（对正常生物、残象皆有效）。“活化”共有两种程度，轻度仅能造成纸上图像在二维平面上轻微移动；重度为画出的图像可以在三维空间中仿真立体形态并留存下来。两种程度“活化”出的对象皆为幻象，非正常实物。本场测试中，幻象最大覆盖范围达到274.5平方米，最长持续时长30分28秒，未能检测出稳定的区间值。<br />
              暂未发现与该对象相似的共鸣频谱检测图，暂时无法确认共鸣源。<br />
              受试样本拉贝尔曲线波动规律、具有显著周期性特征，检测结果判断为先天型共鸣者。
            </p>
          </div>
        </div>
      </section>

      <!-- 新增三大主题区域 -->
      <section id="food" class="card special-section">
        <div class="card-head">
          <h2>写生画笔</h2>
        </div>
        <div class="content two-col">
          <div class="img-area">
            <img src="./img/item1.png" loading="lazy" decoding="async" />
          </div>
          <div class="text-block">
            <p>
              折枝幼时第一次独自出门购物，在老板面前结巴半晌好不容易买下的属于自己的第一套画笔。<br />

              这些画笔陪她走过许多地方，留下了她画技成长的痕迹，即使如今它已陈旧脆弱，无法再被继续使用，折枝依然将其视作可以鼓励自己的珍宝，小心呵护。
            </p>
          </div>
        </div>
      </section>
      <section id="shattered-crystal" class="card special-section">
        <div class="card-head">
          <h2>空鸟笼</h2>
        </div>
        <div class="content two-col">
          <div class="img-area">
            <img src="./img/item2.png" loading="lazy" decoding="async" />
          </div>
          <div class="text-block">
            <p>
              一个空置已久的奢华鸟笼。<br />
              为了奖励折枝听话地用共鸣能力作画，帮他们解决了生意危机，父母自作主张买下鸟儿作为送给她的礼物。折枝看着那只鸟儿在笼边不停冲撞，犹豫许久后，大胆违抗父母严令，打开了那扇门。<br />
              父母失踪后，折枝独自变卖所剩无几的家产，唯独留下了这个早已空无一物的鸟笼。每每看向它时，她看的似乎并不是那道道禁锢，而是那只早已获得自由的鸟儿。
            </p>
          </div>
        </div>
      </section>

      <section id="cyclic-clock" class="card special-section">
        <div class="card-head">
          <h2>愿望清单</h2>
        </div>
        <div class="content two-col">
          <div class="img-area">
            <img src="./img/item3.png" loading="lazy" decoding="async" />
          </div>
          <div class="text-block">
            <p>
              新式甜点、画展门票、跨州写生……常年还债、生活清贫的折枝遇到想买的东西、想做的事情，习惯性先将它们记在自己的愿望清单上。她时常将这份清单拿出来翻看，却只有在偶尔手中钱财有余裕时，才能实现自己的一两个小小愿望
            </p>
          </div>
        </div>
      </section>

      <section id="collective-album" class="card special-section">
        <div class="card-head">
          <h2>糖心锅盔</h2>
        </div>
        <div class="content two-col">
          <div class="img-area">
            <img src="./img/food.png" loading="lazy" decoding="async" />
          </div>
          <div class="text-block">
            <p>
              在锅盔中加入白糖做成的糖心锅盔是折枝最喜欢的食物。这份简单的烤饼香甜可口，短时间内极易补充能量，成本低廉，保质期还长，折枝学会它的做法后，家中常备，外出必备。。
            </p>
          </div>
        </div>
      </section>

      <section
        id="timeline"
        class="card timeline"
        aria-labelledby="timeline-heading"
      >
        <div class="card-head">
          <h2 id="timeline-heading">共鸣链详解</h2>
        </div>
        <div class="tl-wrap" role="list">
          <div class="vl" aria-hidden="true"></div>
          <ol class="tl-list">
            <li
              v-for="(ev, i) in timeline"
              :key="i"
              class="tl-item"
              role="listitem"
              tabindex="0"
              :aria-label="``"
            >
              <div class="tl-card">
                <h3 class="tl-title">第 {{ i + 1 }} 链 · {{ ev.title }}</h3>
                <p class="tl-desc">{{ ev.desc }}</p>
              </div>
            </li>
          </ol>
        </div>
      </section>

      <section class="ll-stories" ref="storyRef">
        <div class="card" role="region" aria-labelledby="stories-title">
          <header class="card-head">
            <h2 id="stories-title" class="card-title">折枝纪事</h2>
          </header>

          <main class="card-body">
            <transition name="fade-slide" mode="out-in">
              <article
                :key="items[current].id"
                :id="`panel-${items[current].id}`"
                class="story"
                role="tabpanel"
                :aria-labelledby="`tab-${items[current].id}`"
              >
                <h3 class="story-head">{{ items[current].title }}</h3>
                <div
                  class="story-content"
                  v-html="items[current].content"
                ></div>
              </article>
            </transition>
          </main>

          <footer class="card-foot">
            <button class="nav-btn" @click="prev" aria-label="上一则故事">
              ‹
            </button>
            <div class="pager">{{ current + 1 }} / {{ items.length }}</div>
            <button class="nav-btn" @click="next" aria-label="下一则故事">
              ›
            </button>
          </footer>
        </div>
      </section>
    </main>
    <div class="floating-chibis">
      <img
        v-for="(pet, i) in chibiList"
        :key="i"
        :src="pet.src"
        :style="{ top: pet.top + 'px', left: pet.left + 'px' }"
        class="chibi-img"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from "vue";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
gsap.registerPlugin(ScrollTrigger);

const timeline = ref([
  {
    title: "骨法用笔",
    desc: `折枝施放共鸣技能极意·神来之笔时，回复15点共鸣能量，且自身暴击提升10%，持续27秒。`,
  },
  {
    title: "气韵生动",
    desc: `共鸣解放虚实境趣的墨鹤的最大召唤数增加6只`,
  },
  {
    title: "应物象形",
    desc: `折枝施放共鸣技能以形写神、共鸣回路神来之笔或共鸣回路极意·神来之笔时，攻击提升15%，可叠加3层，持续27秒。`,
  },
  {
    title: "随类赋彩",
    desc: `折枝施放共鸣解放虚实境趣时，队伍中角色攻击提升20%，持续30秒。`,
  },
  {
    title: "经营位置",
    desc: `共鸣解放虚实境趣持续期间，每召唤3只墨鹤，会额外召唤1只墨鹤协同攻击，能造成等同于墨鹤140%的伤害，此次伤害为普攻伤害，该伤害不会触发召唤墨鹤的效果。`,
  },
  {
    title: "传移摹写",
    desc: `施放共鸣技能神来之笔或共鸣技能极意·神来之笔时，会额外召唤一只白鹤，能造成等同于神来之笔120%的伤害，此次伤害为普攻伤害。`,
  },
]);

const reducedMotion =
  window.matchMedia &&
  window.matchMedia("(prefers-reduced-motion: reduce)").matches;

const items = [
  {
    id: "1",
    title: "掌上明珠",
    content: `女孩将自己手中的画举到母亲面前，开心地分享自己的成果。专心搭配首饰的女人瞥了一眼画面，脸上露出短暂的笑容，“天啊，折枝，你画得真好。”女孩听到夸赞，雀跃地贴近自己的母亲，“这张画里的人和之前的……”她的话还没说完，女人已经走到衣柜前开始挑选包装自己的衣服，完全没在意身后女孩的亲近，“很好，你进步很大！客人来之前自己再多画几张好吗？”女人见她不走，不耐烦的将她推了出去，“画材有什么缺的直接买就好。无论多名贵的东西，咱们都买得起。乖，去画吧。”一声干脆的关门声后，折枝看着面前紧闭的大门，还想说些什么，可她想了一会，最终没有勇气再次敲响它。

面对满堂宾客，衣着华丽的女孩攥着裙角，躲在角落的座位上默不作声。女人看着她局促的样子，似乎再次看到了他们贫困落魄的过去、无趣廉价的灵魂。她立即走过去，拉着女孩的手将她带到了人群中央。有天赋的女儿是他们炫耀的资本，也是他们成为真正人上人的最后希望。“我们家折枝确实从小就有天赋，没给她请老师之前，她的画就已经被选去参加画展了。”母亲脸上带着得意的笑意，举手投足间都是为融入更高阶层而模仿出的高贵与优雅，“您也想要一张画？那有什么问题，折枝不到一天就能画完。什么主题都可以，放心交给她就行。”折枝站在母亲身后，听到这话忍不住悄悄扯了扯她的衣服，却没想到母亲将她直接推了出去，“快，折枝，给大家展示一下你的画技。”拥挤的目光齐刷刷集中到了她的身上，折枝想退却无路可退。她拿着画笔的手第一次有些颤抖，久久无法落墨。

直到深夜，偌大的宅邸才终于重归沉寂。折枝重新鼓起勇气，拿着白天的那幅画跑到父母房门前，抬手敲门前却听到了屋里的谈话。

“你知道吗，今天那张画卖了四十万钱。有了这笔钱，那个被迫暂停的订单就可以继续了。”

“对了，这周末要带给客人的画应该都准备好了吧？”

“还没有呢。折枝前两天和我说她想去看戏。真是的…只想着享乐，一点都不愿意为这个家付出……”

站在门外的折枝低头看着画纸上幸福的一家三口，攥着画纸的手渐渐收紧，直到纸张起皱，扭曲了父母的脸。

“……对不起。”`,
  },
  {
    id: "2",
    title: "画的真意",
    content: `折枝出门写生后才发现自己笔下的画面似乎还缺什么。女孩望着眼前的田园风光，突然有了个新的想法。她小心地环顾四周，发现无人后便大胆地伸了个懒腰，暂时将父母对她提出的各种要求抛之脑后。

赤脚踏上草地的那一刻，刺刺的痒意、微凉的柔软让她感受到了自然的生机。她肆意地跑了几步，不再去想自己的举止是否足够高贵，以至于不会给家里丢脸。

就在这时，一个声音突然从远处传来。

“喂，你是谁？你在我们家菜地里干什么？”

折枝听到声音，立刻看了一眼脚下，尴尬地跑开，连连道歉，“对不起！对不起！我、我是来画画的，我不知道那里是田地……”

女孩看她不认识地里种的菜，慌张离开时还差点踉跄摔倒，忍不住笑出了声，“哈哈哈！这些菜压不坏的，你别跑了！”她说完拿着手里的镰刀一边笑一边收起菜来，“画画的人，真稀奇……”折枝犹豫了片刻，忍不住走过去有些愧疚地说道：“我帮你吧……”女孩抬起头，笑着点头，“好啊！谢谢！这把镰刀给你用，我再去拿一把！”

折枝看着塞到自己手里的镰刀，又看了看地上的菜，蹲下身学着女孩的样子握紧镰刀，刀还未落，就被跑回来的女孩叫住了，“不对不对，这样收很慢的，我教你怎么用！”

两个小小的身影互相支持着在阳光下缓缓前行，直到走到田地的尽头。她们累得躺倒在树荫下，昂贵的衣服沾满了泥土，折枝浑然不觉，只顾和新朋友畅聊趣事。这样无所顾忌的说笑让她感觉异常轻松、快乐。

一道尖利的责备声突然响起，打断了她们的交谈。看到愤怒的母亲正站在不远处盯着她们的一举一动，鄙夷嫌弃的目光刺向旁边的女孩，折枝连忙跑回去道歉、解释，在一阵严厉的数落声中重新将自己规整成了乖巧端庄的模样。

多日后，女孩收到了一个卷轴。她好奇地打开后，立刻笑出了声。

画面上不仅有她家周围的美丽风景，还记录下了那天二人在田地间收菜的样子。

夹在画中的信轻轻掉落。

“很抱歉上次没能和你道别，和你一起收菜很开心！镰刀的重量、阳光打在背上的温度、手掌的酸痛……这一切真实的感受让我明白我的画欠缺的是什么了。这是我至今为止画得最好的一张画，送给你！希望你能喜欢！如果下周还有机会，我再偷偷溜出来帮你收菜。”

女孩笑着读过每一行，直到看到最后一句，她挠了挠脑袋。

下周？没有要收的菜啊……难道，她觉得那些菜下周就会再长出来吗……`,
  },

  {
    id: "3",
    title: "或将不再踯躅",
    content: `折枝站在画材店不远处，看到人高马大的店员面色不善，正紧皱着眉头记录着什么，她犹豫了一下，向后退了几步。

要不还是去别的店买吧……脑袋里刚冒出退缩的想法，就立刻被自己否决了。但是…这个新品别的店还没有……

在不远处徘徊的女孩停下了脚步，习惯性地拿出了一枚贝币。她将贝币抛向空中，很快接住，心里默念。

老规矩，如果是正面，我就进去看看，如果是背面，就下次再说吧……

折枝慢慢打开手心，看到结果后，立刻攥紧。

不行不行，还是三局两胜吧。

贝币再一次从空中落入手中。

……还是一样的结果啊。

折枝攥紧手中贝币，深呼吸之后双眼变得坚毅。

这一定是一种指引，暗示我不能总是逃避下去。

她鼓足勇气，坚定地走向店里，用力推开门后，头顶铃铛清脆的声音让她顿时紧张到了极点。她低着头，舌头着火似的快速把心里默念了无数遍的句子说了出来。

“你、你好！我想买新到的颜料，六罐三绿、五罐三青、五罐大红、三罐花青、三罐赭石、三罐藤黄，两罐鹅黄，谢谢！”

顺畅无误的说完后，折枝长呼一口气。她开心地抬起头，却正好与刚从后面房间走过来的店员四目相对。

“亲亲您好，请问您需要什么吗？”

店员有些疑惑地看着不知何时出现在店里的客人，语气是与折枝想象中完全不同的温和亲切。

折枝看着刚出现的店员，意识到刚才的话其实除了自己没有任何人听到，她一时间僵在了原地。

“呃…我……”

“您是需要购买什么画材吗？”

“……颜…料…那个……”

“新品颜料吗？最近很多人来买呢。”

“不不…我还是……”

“没关系您可以慢慢看的。”

看到店员伸出手热情的邀请，折枝攥紧渗出冷汗的双手，硬着头皮走到了货架旁，“好…好的……”

在店员细心、专业的介绍下，认真听着的折枝慢慢放松了下来，重新说出了自己的需求，甚至还多聊了些绘画上的习惯。

“如果搭配白玉堂纸的话，推荐您也试试这几款新品，效果也很好。”

“太好了，这正好符合我的需求。那…这几支我也试试看吧。”

心满意足拎着颜料走出店铺的折枝脚步轻盈，对自己的信心都上涨了起来。

放松下来还是可以好好和别人说清楚的，下一次…试试看不抛贝币吧，说不定也能成功呢……`,
  },

  {
    id: "4",
    title: "折叠世界",
    content: `毫不知情的少女被逐出了家门。她看着被丢在院子的凌乱杂物，才知道自己和它们一样，被父母丢弃在了这里。

流落街头的她本能地寻找着自己熟悉的庇护所。过去无比亲切的人在得知她的状况时，脸上的笑容消失无影，只将街角那幢摇摇欲坠的旅馆指给了她。

腐朽潮湿的走廊中滋生着她从未见过的争吵与斗殴。坐在床上的少女裹紧发霉的被子，不安地看着那扇被桌子抵死的单薄木门。她想要睡去，希望醒来发现这一切不过是一场噩梦。她不敢睡去，因为她害怕自己一闭上眼，那扇木门就将支离破碎。

由奢入俭具象化为每一个日夜，凝聚成了一段漫长而痛苦的过程。

昏暗的灯在没有窗户的房间中忽明忽暗，让还债清单上的那一串串天文数字不停的躁动。那双画画的手上绑着陈旧的绷带，即使手指已经开始微微颤抖，连画了两天两夜的折枝依旧专注于画面，不敢停下。

好不容易接到大单子，一定要画好……

困倦的双眼逐渐模糊，画面上的山水开始出现重影。

激烈的叫嚷在深夜中格外刺耳，不知何时趴在桌子上睡着了的折枝被粗暴的敲门声惊醒。她起身跑去开门，却被没过膝盖的冷水绊倒。

浑身湿透的人不安地站在漏水的房前，不停向邻居们道歉。

刺骨的寒冷经过衣裳穿透她的身体，邻居的抱怨声隔着薄墙依旧回荡在耳边，折枝静静看向桌上的画，看着画了无数个日夜的画面在水中被毁于一旦。

一声沉重的叹气回荡在幽暗的房间中，逐渐挤压掉稀薄的空气，即将压垮那副瘦削的肩膀。

疲倦的背影伫立在冷水中，沉默了很久。

许久之后，微弱的光亮在狭促的屋子里亮起。

伤痕累累的手在终端上艰难起落，“先关闭水阀…再替换受损部分……”

她习惯性地搜索着自己没遇到过的情况，不断了解着生活中未曾面对过的一切。

“大概明白了…试试看吧……”

她淌过急流，缓缓走向问题的源头。`,
  },

  {
    id: "5",
    title: "一束光的到来",
    content: `“我要让所有人第一眼看到这幅画就知道它很贵，这样才衬托得起我的身份。”

“就这种成稿质量……你给我打个五折，这幅画我就勉强收下了。”

委托人的话在脑海中挥之不去，折枝抱着无数画卷，浑浑噩噩地走在街上，像具失去灵魂的空壳，完全没注意到周围正在布置节庆活动。直到被地上的木材绊倒，画卷散落一地。跌到地上的人才醒过神来，不顾寻找眼镜，急忙拿回画卷。避开忙碌的人们，收回身边几幅画后，她看到剩余的作品被风吹着滚落远处，疲惫的人渐渐停下了动作，不再试图挽救。

算了，这么差劲的画…拿回来又有什么用……

没有人真正好好看看它们……

她松开了双手，让怀里仅存的几幅画也滚到了地上，低头沮丧之时，却没注意到有个人停在了她的面前。

陌生人将画卷与眼镜一起收了回来，递给了她。

“这些画是你画的吗？形神兼备，柔中带刚，画得真好。”

声音从头顶传来，接过物品的折枝听到这句话诧愕不已。她连忙抬头，还没来得及说什么，却发现说话之人放下东西后早已和同伴一起，在夜色中渐行渐远。

她手忙脚乱地戴好眼镜，看向远去的人，却只看到模糊的黑衣背影。

有人赞美了我的画……

不是为了趋炎附势的社交，也没有撕扯不尽的利益，只是在夸奖我的画……

那句话穿过画面看透了她的内心，真正懂得她的创作，懂得她所在意的一切，像是突如其来的一束光，驱散了内心深处积聚多时的乌云，久违的让她感受到了温暖。

她看向那个身影早已消失的方向，渐渐抱紧了怀里的画。

“打扰了……你好？”

藏在心底的声音突然在身边响起，让沉迷于取景的折枝一时恍然。

“别紧张，我们没有恶意……”

那道声音的主人，在这一刻，从一个模糊的黑衣背影渐渐变得清晰。

她回头望向那双金瞳，回想起了街上偶遇的夜晚：在她身前半蹲下来的人，语气里带着一样温柔的笑意，说话时短暂创造了一片安全的氛围，及时将她拉出了自我怀疑的无底漩涡。

深夜中的那股温暖再次涌上心头，让她眼前浅淡黑白的水墨世界，由此染上色彩。
`,
  },
];
const current = ref(0);

function select(index) {
  current.value = index;
  // 将焦点移回当前 tab（便于无障碍与键盘导航）
  const id = `tab-${items[index].id}`;
  const el = document.getElementById(id);
  if (el) el.focus();
}
const storyRef = ref<HTMLElement | null>(null);

function prev() {
  current.value = (current.value - 1 + items.length) % items.length;
  select(current.value);
  storyRef.value.scrollIntoView({ behavior: "smooth", block: "start" });
}

function next() {
  current.value = (current.value + 1) % items.length;
  select(current.value);
  storyRef.value.scrollIntoView({ behavior: "smooth", block: "start" });
}
interface Chibi {
  src: string;
  top: number;
  left: number;
}

const chibiList = ref<Chibi[]>([]);
onMounted(async () => {
  if (!reducedMotion) {
    const tl = gsap.timeline({ defaults: { ease: "power3.out" } });
    tl.from(".avatar-card", {
      duration: 0.8,
      y: 18,
      opacity: 0,
      scale: 0.99,
    }).from(
      ".right-col > .hero-panel .name",
      { duration: 0.9, x: 24, opacity: 0 },
      "-=0.45"
    );

    gsap.utils.toArray(".tl-item").forEach((it: any, idx: number) => {
      gsap.from(it.querySelector(".tl-card"), {
        scrollTrigger: {
          trigger: it,
          start: "top 85%",
          toggleActions: "play none none reverse",
        },
        opacity: 0,
        y: 22,
        duration: 0.7,
        delay: idx * 0.06,
      });
    });

    gsap.to(".avatar-card", {
      scale: 1.007,
      duration: 2.8,
      yoyo: true,
      repeat: -1,
      ease: "sine.inOut",
    });
  } else {
    document.documentElement.classList.add("reduced-motion");
  }

  // 1. 基础配置信息
  const total = 6
  let pickCount = 3; // 每次抽取 3 张
  const vw = window.innerWidth;
  const vh = window.innerHeight;
  const isMobile = window.innerWidth <= 768;
  // 如果已知单张小人图片的宽高，可避免超出边界；
  // 假设小人图片宽 100px、高 100px，按需替换：
  const imgWidth = 100;
  const imgHeight = 100;

  // 2. Fisher–Yates 洗牌函数
  function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
  }

  // 3. 随机选出 3 个编号
  if (isMobile) {
    pickCount = 1;
  }
  const nums = shuffle(Array.from({ length: total }, (_, k) => k + 1));
  const picks = nums.slice(0, pickCount);

  // 4. 生成随机位置并填充 chibiList
  chibiList.value = []; // 先清空
  picks.forEach((i) => {
    chibiList.value.push({
      src: `/QImages/1 (${i}).png`,
      left: Math.random() * (vw - imgWidth), // 保证不超出左右边界
      top: Math.random() * (vh - imgHeight), // 保证不超出上下边界
    });
  });

  // 2. 等 img 渲染到 DOM
  await nextTick();

  // 3. 给每个小人绑定 GSAP 动画
  const imgs = document.querySelectorAll<HTMLImageElement>(".chibi-img");
  imgs.forEach((img, index) => {
    const padding = 200; // 边缘预留空间
    // ✅ 初始出场动画（闪现）
    gsap.fromTo(
      img,
      { opacity: 0, scale: 0.5 },
      {
        opacity: 1,
        scale: 1,
        duration: 0.8,
        ease: "back.out(2)",
        delay: 0.2 * index,
      }
    );

    // ✅ 鼠标靠近闪避
    img.addEventListener("mouseenter", () => {
      gsap.killTweensOf(img);

      gsap.to(img, {
        x: "+=" + ((Math.random() - 0.5) * 400).toFixed(0),
        y: "+=" + ((Math.random() - 0.5) * 400).toFixed(0),
        duration: 1.2,
        ease: "back.out(2)",
        onComplete: () => {
          // 闪避完成后，再重新启用动画
          animate(img);
        },
      });
    });

    const animate = (img: HTMLImageElement) => {
      let { x, y } = img.getBoundingClientRect();
      let deltaX = (Math.random() - 0.5) * 200;
      let deltaY = (Math.random() - 0.5) * 200;

      // 预测一下偏移后的位置
      let nextX = x + deltaX;
      let nextY = y + deltaY;

      // 校正：防漂出左、右、上、下边界
      if (nextX < padding) deltaX = padding - x;
      if (nextX + img.width > window.innerWidth - padding)
        deltaX = window.innerWidth - padding - (x + img.width);
      if (nextY < padding) deltaY = padding - y;
      if (nextY + img.height > window.innerHeight - padding)
        deltaY = window.innerHeight - padding - (y + img.height);

      gsap.to(img, {
        x: `+=${deltaX.toFixed(0)}`,
        y: `+=${deltaY.toFixed(0)}`,
        rotation: `+=${((Math.random() - 0.5) * 60).toFixed(0)}`,
        duration: 2 + Math.random() * 2,
        ease: "power1.inOut",
        onComplete: () => animate(img),
      });
    };
    animate(img);
  });
});
</script>

<style scoped lang="scss">
/* 折枝风格 —— 紫蓝海感（水母/幻海） + 毒药/治愈双重高光 */
$bg-deep: #f6f3ee;
$deep-2: #efe7dc;
$accent-1: #2e8aa3;
$accent-2: #f3a6b8;

$text-main: #1b1d20; // 文字

$card-bg: rgba(238, 238, 238, 0.46);
$card-border: rgba(184, 107, 224, 0.06);
$soft-shadow: rgba(0, 0, 0, 0.6);
$inner-glow: rgba(184, 107, 224, 0.03);
/* 固定浮动小 chibi 层（保留） */
.floating-chibis {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 1;
  .chibi-img {
    position: absolute;
    width: 80px;
    user-select: none;
    transform-origin: center center;
    pointer-events: auto;
    z-index: 10;
  }
}

.megumi-page {
  position: relative;
  min-height: 100vh;
  background: linear-gradient(180deg, $bg-deep 0%, $deep-2 72%);
  font-family: "Noto Sans SC", system-ui, -apple-system, "Segoe UI", Roboto,
    Arial;
  color: $text-main;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: clamp(20px, 4vw, 80px);
  box-sizing: border-box;
  padding-top: 60px;

  &.reduced-motion * {
    transition: none !important;
    animation: none !important;
  }

  .container {
    position: relative;
    z-index: 10;
    max-width: 1100px;
    margin: 0 auto;
    padding: clamp(12px, 2vw, 40px);
  }
}

/* ========== HERO 区域（整体暗化 + 血玫点光） ========== */
.hero {
  position: relative;
  display: grid;
  grid-template-columns: 280px 1fr;
  gap: 28px;
  align-items: start;
  padding: 12px;
  margin-bottom: 12px;

  /* 乐谱画布 —— 在 hero 背后增加五线谱移动感 */
  &::before {
    content: "";
    position: absolute;
    inset: 0;
    z-index: 0;
    pointer-events: none;
    background-image: repeating-linear-gradient(
      to bottom,
      rgba(255, 255, 255, 0.02) 0px,
      rgba(255, 255, 255, 0.02) 1px,
      transparent 1px,
      transparent 16px
    );
    opacity: 0.06;
    mix-blend-mode: overlay;
    transform: translateY(-4%);
    animation: staff-drift 16s linear infinite;
  }

  .left-col {
    display: flex;
    flex-direction: column;
    align-items: center;
    z-index: 6;
  }

  .avatar-card {
    background: linear-gradient(
      180deg,
      rgba(255, 255, 255, 0.01),
      rgba(184, 107, 224, 0.01)
    );
    border-radius: 16px;
    padding: 12px;
    display: grid;
    justify-items: center;
    gap: 8px;
    border: 1px solid $card-border;
    box-shadow: 0 14px 44px $soft-shadow, inset 0 1px 0 $inner-glow;
    cursor: pointer;
    transition: transform 0.28s ease, box-shadow 0.28s ease;
    width: 100%;
  }

  .avatar-wrap {
    width: 100%;
    display: block;
  }

  .avatar {
    width: 100%;
    height: auto;
    border-radius: 12px;
    display: block;

    transition: transform 0.5s ease, filter 0.5s ease;
    background-color: rgba(0, 0, 0, 0.04);
    filter: saturate(0.8) contrast(0.95) brightness(0.92);
  }

  .avatar-card:hover .avatar {
    transform: translateY(-4px) scale(1.02);
    filter: saturate(0.7) contrast(0.9) brightness(0.9);
  }

  .avatar-meta {
    display: flex;
    gap: 8px;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
  }

  .avatar-badge {
    font-size: 0.9rem;
    color: $text-main;
    background: rgba(0, 0, 0, 0.18);
    padding: 6px 10px;
    border-radius: 999px;
    border: 1px solid rgba($accent-2, 0.05);
    font-weight: 600;
  }

  .right-col {
    display: flex;
    flex-direction: column;
    gap: 12px;
    z-index: 6;
  }

  .hero-panel {
    position: relative;
    z-index: 6;
    padding: 20px;
    border-radius: 14px;
    background: $card-bg;
    backdrop-filter: blur(8px) saturate(0.9);
    border: 1px solid $card-border;
    box-shadow: 0 12px 50px rgba(0, 0, 0, 0.6), inset 0 1px 0 $inner-glow;
  }

  .name {
    font-size: clamp(1.8rem, 3vw, 2.8rem);
    margin: 0;
    font-weight: 900;
    letter-spacing: 1px;
    line-height: 1;
    display: inline-block;
    /* 标题改为 暗紫 → 血玫 渐变 */
    background: linear-gradient(90deg, $accent-1 0%, $accent-2 70%);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    color: transparent;
    -webkit-text-stroke: 0.2px rgba(0, 0, 0, 0.22);

    position: relative;
  }

  .jp {
    font-size: 0.95rem;
    color: $accent-1;
    margin-left: 8px;
    font-weight: 400;
  }

  .subtitle {
    margin: 6px 0 0;
    font-size: 1.1rem;
    color: $accent-1;
    font-weight: 600;
    line-height: 1.4;
  }

  .lead {
    margin: 10px 0 0;
    color: $text-main;
    opacity: 0.96;
    line-height: 1.7;
    max-width: 72ch;
  }
}

/* ========== 通用卡片（更暗、更锐利的边与血色提示） ========== */
.card {
  margin-top: 24px;
  background: rgba(240, 240, 240, 0.5);
  border-radius: 14px;
  padding: 24px;
  box-shadow: 0 12px 48px rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(6px);
  color: $text-main;
  border: 1px solid $card-border;
  transition: transform 0.28s ease, box-shadow 0.28s ease;

  &:hover {
    transform: translateY(-8px) scale(1.01);
    box-shadow: 0 30px 90px rgba(0, 0, 0, 0.7);
  }

  h2 {
    margin: 0 0 12px 0;
    font-size: 1.3rem;
    color: $accent-2;
  }

  .card-head {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 12px;
  }
}

/* personality 列表样式（暗紫细线） */
.personality .traits li {
  padding: 12px 0;
  border-bottom: 1px dashed rgba($accent-1, 0.06);
  font-size: 1rem;
  color: rgba($text-main, 0.92);
}

/* overview 双栏布局（保持） */
.overview .two-col {
  display: flex;
  gap: 24px;
  flex-wrap: wrap;
  margin-top: 12px;
}

.overview .text-block {
  flex: 1 1 40%;
  min-width: 240px;
  line-height: 1.6;
  color: rgba($text-main, 0.96);
}

/* special-section：加入音符漂浮和暗紫边框 */
.special-section {
  position: relative;
  overflow: hidden;

  .content {
    display: flex;
    gap: 24px;
    align-items: center;
    margin-top: 16px;
  }

  .img-area {
    flex: 0 0 200px;

    img {
      width: 100%;
      height: auto;
      border-radius: 12px;

      border: 1px solid rgba($accent-1, 0.03);
    }
  }

  .text-block {
    flex: 1;
    line-height: 1.6;
    color: rgba($text-main, 0.95);
  }
}

/* ll-stories — 暗底 + 微血色分割线 */
.ll-stories {
  .card {
    width: min(1200px, 100%);
    border-radius: 12px;

    color: $text-main;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }

  .card-head {
    padding: 14px 16px;
    border-bottom: 1px solid rgba($accent-2, 0.03);
    display: flex;
    flex-direction: column;
    gap: 12px;

    .card-title {
      font-size: 24px;
      font-weight: 700;
      color: $accent-1;
      letter-spacing: 0.6px;
    }
  }

  .card-body {
    padding: 18px 16px;
    min-height: 160px;
    position: relative;
    background: linear-gradient(180deg, rgba(0, 0, 0, 0), rgba(8, 8, 10, 0.02));
  }

  .story {
    color: rgba($text-main, 0.96);

    .story-head {
      font-size: 18px;
      color: $accent-2;
      font-weight: 700;
      margin-bottom: 8px;
    }

    .story-content {
      line-height: 1.7;
      color: rgba($text-main, 0.92);
      white-space: pre-wrap;
    }
  }

  .card-foot {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 8px;
    padding: 10px 14px;
    border-top: 1px solid rgba($accent-1, 0.02);
    background: linear-gradient(
      180deg,
      rgba(0, 0, 0, 0.02),
      rgba(0, 0, 0, 0.04)
    );

    .nav-btn {
      background: rgba($text-main, 0.6);
      border: 1px solid rgba($accent-1, 0.06);
      color: $accent-2;
      padding: 6px 10px;
      border-radius: 6px;
      cursor: pointer;
      transition: background 180ms ease;
      &:hover {
        background: rgba(#000, 0.9);
      }
    }

    .pager {
      color: rgba($text-main, 0.72);
      font-size: 13px;
      font-weight: 600;
    }
  }

  /* 切换动画保留，但更暗 */
  .fade-slide-enter-active,
  .fade-slide-leave-active {
    transition: all 320ms cubic-bezier(0.2, 0.9, 0.2, 1);
  }

  .fade-slide-enter-from {
    opacity: 0;
    transform: translateY(6px);
  }

  .fade-slide-enter-to {
    opacity: 1;
    transform: translateY(0);
  }

  .fade-slide-leave-from {
    opacity: 1;
    transform: translateY(0);
  }

  .fade-slide-leave-to {
    opacity: 0;
    transform: translateY(-6px);
  }
}

/* timeline（暗紫竖线 + 微光） */
.timeline {
  position: relative;

  .tl-wrap {
    position: relative;
    padding: 24px 12px 12px 96px;
  }

  .vl {
    position: absolute;
    left: 80px;
    top: 32px;
    bottom: 20px;
    width: 4px;
    border-radius: 6px;
    background: linear-gradient(
      180deg,
      rgba($accent-2, 0.12),
      rgba($accent-1, 0.1)
    );
    box-shadow: 0 6px 18px rgba($accent-1, 0.06),
      inset 0 0 10px rgba($accent-1, 0.02);
    opacity: 0.95;
  }

  .tl-list {
    list-style: none;
    margin: 0;
    padding: 0;
  }

  .tl-item {
    position: relative;
    display: flex;
    gap: 18px;
    align-items: flex-start;
    margin: 20px 0;
    transition: transform 0.28s ease, box-shadow 0.28s ease;
  }

  .tl-card {
    border-radius: 12px;
    padding: 16px 20px;

    width: calc(100% - 130px);
    border-left: 4px solid transparent;
    transition: transform 0.28s ease, box-shadow 0.28s ease,
      border-color 0.28s ease;
    position: relative;
    z-index: 2;
  }

  .tl-item:hover .tl-card,
  .tl-item:focus-within .tl-card {
    transform: translateY(-8px);
    box-shadow: 0 24px 72px rgba(0, 0, 0, 0.6);
    border-left-color: rgba($accent-2, 0.18);
  }

  .tl-title {
    margin: 0 0 6px 0;
    font-size: 1.1rem;
    color: $accent-1;
    display: flex;
    align-items: baseline;
    gap: 8px;
  }

  .tl-desc {
    margin: 0;
    color: rgba($text-main, 0.92);
    white-space: pre-wrap;
  }
}

/* 浮动音符（可在模板中渲染若干 .floating-note） */
.floating-note {
  position: absolute;
  font-size: 14px;
  color: $accent-2;
  opacity: 0.95;
  transform-origin: center;
  animation: note-float 4.6s ease-in-out infinite;
  filter: drop-shadow(0 6px 18px rgba($accent-2, 0.06));
}

/* 动画关键帧 */
@keyframes staff-drift {
  0% {
    transform: translateY(-4%);
    opacity: 0.9;
  }
  50% {
    transform: translateY(6%);
    opacity: 0.95;
  }
  100% {
    transform: translateY(-4%);
    opacity: 0.9;
  }
}

@keyframes note-float {
  0% {
    transform: translateY(0) rotate(-6deg) scale(0.95);
    opacity: 0.85;
  }
  50% {
    transform: translateY(-12px) rotate(3deg) scale(1);
    opacity: 1;
  }
  100% {
    transform: translateY(0) rotate(-6deg) scale(0.95);
    opacity: 0.85;
  }
}

/* 响应式：小屏适配（保留原逻辑，仅小幅调整文字色调与阴影） */
@media (max-width: 980px) {
  .hero {
    grid-template-columns: 1fr;
    gap: 18px;
    padding: 8px;
  }

  .avatar-card {
    width: 140px;
    padding: 8px;
  }

  .hero-panel {
    padding: 14px;
  }

  .name {
    font-size: 1.8rem;
  }

  .subtitle {
    font-size: 1rem;
  }

  .lead {
    font-size: 0.95rem;
  }

  .card {
    padding: 18px;
    border-radius: 12px;
  }

  .overview .two-col {
    flex-direction: column;
  }

  .special-section .content {
    flex-direction: column;
  }

  .ll-stories {
    .card {
      border-radius: 10px;
    }

    .card-head {
      padding: 12px 12px;
    }

    .card-body {
      padding: 14px 12px;
      min-height: 180px;
    }

    .card-foot {
      padding: 8px 12px;
    }
  }

  .timeline {
    overflow: visible;
  }

  .timeline .tl-wrap {
    padding-left: 14px !important;
    padding-right: 14px;
    min-height: 0;
  }

  .timeline .vl {
    display: none !important;
  }

  .timeline .tl-list {
    overflow: visible;
  }

  .timeline .tl-item {
    display: block !important;
    margin: 14px 0 !important;
    width: 100%;
  }

  .timeline .tl-card {
    width: 100% !important;
    padding: 14px !important;
    border-left: 0 !important;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.45);
    transform: none !important;
  }
}
</style>

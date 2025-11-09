---
title: CDS2019 Next-generation web styling 整理介紹
date: 2019-12-30 19:32:59
tags:
author: ArvinH
---

# 前言

一陣子沒有關注 CSS 的最新發展，最近趁著年假補帶 2019 Google Chrome Summit 時，看到這場 talk - [Next-generation web styling](https://youtu.be/-oyeaIirVC0)，裡面提到不少有趣的新屬性，透過這篇文章整理一下內容，分享給各位與未來的我。
註：官方在 12 月初也發佈了[文字版本](https://web.dev/next-gen-css-2019/)，習慣閱讀原文的讀者可以參考看看。

# 新世代的...CSS

1. scroll-snap - native scroll inertia and decelerations
2. focus-within - solving focus accessibility within elements
3. @media (prefers-\*) - considerately adjust your UI/UX to a user's device preferences via Media Query hooks provided by browser
4. logical properties - dynamic directionality
5. sticky situations - keeping UI within the viewport
6. backdrop-filter - style adjustments behind an element
7. :is() - formerly :any() && :matches()
8. Grid-gap
9. CSS Houdini - a low-level API for CSS
10. typed OM - CSS values as JavaScript objects rather than strings
11. Paint API - create your own paint functions using a canvas-like syntax
12. animation worklet - off the main thread animation
13. Others - size, aspect-ratio, min(), max(), clamp(), list style type, display: outer inner, CSS regions, CSS modules

演講中洋洋灑灑介紹了 12 種以上的 CSS 新屬性與新功能，雖然其中有幾位老朋友在我之前的文章 - [CSS 魔術師 Houdini API 介紹](https://blog.techbridge.cc/2017/05/23/css-houdini/) 有說明過，但大部分的新屬性還是令人感到振奮！

## scroll-snap

看到 snap 自然反應先想到薩諾斯彈指消滅一半人口，但其實字面上還有迅速回復的意思，而 scroll-snap，顧名思義就是要讓你在 scroll 時，能夠迅速回到設定的 snap point。這麼說有點抽象，回想一下你使用過的 Carousel 套件，大多數當你滑動圖片超過一半距離時，套件會自動幫你將下一張圖片拉到正中間，若是滑動小於一半距離，則會彈回至原本的圖片，就像這樣：

![Demo](https://static.coderbridge.com/img/ArvinH/f271a7cbda1641efa2ecc6dda50d27cf.gif)

scroll-snap 主要提供的兩個屬性：`scroll-snap-type` 與 `scroll-snap-align` 就可以讓你直接透過 CSS 達到一樣效果，而且擁有足夠的調整彈性。

Demo：

<p class="codepen" data-height="430" data-theme-id="29194" data-default-tab="css,result" data-user="arvin0731" data-slug-hash="Exawayy" style="height: 430px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="scroll-snap example">
  <span>See the Pen <a href="https://codepen.io/arvin0731/pen/Exawayy">
  scroll-snap example</a> by Arvin (<a href="https://codepen.io/arvin0731">@arvin0731</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

`scroll-snap-type` 作用在 scroll container 上頭，用來表明要用哪種 type 的 scroll 與 scroll 的方向。（註：scroll container 　指的是擁有 `overflow: scroll|auto` 屬性，且其內元素足以造成 overflow 的 container。）

而比起上述一般 carousel 套件預設的行為（移動小於一半距離拉回前一張圖），透過給予 `scroll-snap-align` 不同的屬性：`start`、`end` 和 `center`，可以自行決定 snap point，告知內容應該要對齊到 scroll container 中的哪個點。

你可以從上面的 Codepen demo link 去修改 `scroll-snap-align` 看看，有什麼不同結果。

另外，如果你不想讓每次 snap 都對齊邊緣，能稍微露出前一張圖的內容，可以透過 `scroll-padding` 與 `scroll-margin` 的調整來達成。不過注意，兩者的應用對象有所不同。

`scroll-padding` 需要應用在 scroll container 上：

```css
.scroller {
  height: 300px;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
  scroll-padding: 40px;
}

.scroller section {
  scroll-snap-align: start;
}
```

而 `scroll-margin` 則是運用在 container 內的 children 上頭：

```css
.scroller {
  height: 300px;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
}

.scroller section {
  scroll-snap-align: start;
  scroll-margin: 40px;
}
```

兩者效果相同：

<p class="codepen" data-height="428" data-theme-id="29194" data-default-tab="css,result" data-user="arvin0731" data-slug-hash="jOEGbdQ" style="height: 428px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="scroll-snap example-2">
  <span>See the Pen <a href="https://codepen.io/arvin0731/pen/jOEGbdQ">
  scroll-snap example-2</a> by Arvin (<a href="https://codepen.io/arvin0731">@arvin0731</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

這對於你的 scroll container 中有 fixed 的元素時很有幫助：

<p class="codepen" data-height="420" data-theme-id="29194" data-default-tab="css,result" data-user="arvin0731" data-slug-hash="NWPaGmL" style="height: 420px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="scroll-snap example-fixedHeader">
  <span>See the Pen <a href="https://codepen.io/arvin0731/pen/NWPaGmL">
  scroll-snap example-fixedHeader</a> by Arvin (<a href="https://codepen.io/arvin0731">@arvin0731</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

更多細節，[MDN 上有蠻完整的說明](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Scroll_Snap/Basic_concepts)。

![scroll-snap-caniuse](https://static.coderbridge.com/img/ArvinH/10d10829177d46adab0c0ee8d582b217.png)

## focus-within

在[Web Accessibility 的重要性](https://blog.techbridge.cc/2019/10/13/web-accessibility-intro/)這篇文章中，有提到過元素可聚焦性的重要，讓使用者能用 tab 在網頁各元素間切換。但實作上常常遇到一個問題，就是當我們利用 `:focus` 僞類別製作 Menu 的下拉選單，讓子元素在父元素被 focus 後顯示出來時，tab 切換會失敗，因為當你 tab focus 到子元素時，父元素就失去 focus 狀態，子元素也因應消失：

[google 的圖畫](https://web.dev/next-gen-css-2019/#:focus-within)說明很清楚：

<img src="https://static.coderbridge.com/img/ArvinH/3f70bc94fb30417c8a5a2b5bd83b6b5e.png" style="max-width: 700px" />

但若是換成 `focus-within`，就不再有這問題了，他會在父元素被 focus 時觸發：

```css
.menu:focus-within {
  display: block;
  opacity: 1;
  visibility: visible;
}
```

<p class="codepen" data-height="300" data-theme-id="29194" data-default-tab="css,result" data-user="arvin0731" data-slug-hash="abzLBrP" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="focus-within-example">
  <span>See the Pen <a href="https://codepen.io/arvin0731/pen/abzLBrP">
  focus-within-example</a> by Arvin (<a href="https://codepen.io/arvin0731">@arvin0731</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

![caniuse-focuswithin](https://static.coderbridge.com/img/ArvinH/9af24cbb0e80433b8d43b184c76822d6.png)

## @media (prefers-\*)

Media Queries 讓我們容易實作 RWD 設計，而現在最新的 Media Queries 可以讓我們偵測到使用者 OS-level 的系統偏好設定，例如 dark-mode，或是低階設備可以開啟 `prefers-reduced-motion` 來降低動畫節省資源。

這些能夠自動偵測系統偏好，並給予回應的 Media Queries 有：

[prefers-reduced-motion](https://developers.google.com/web/updates/2019/03/prefers-reduced-motion)
[prefers-color-scheme](https://web.dev/prefers-color-scheme/)
[prefers-contrast](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-contrast)
[prefers-reduced-transparency](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-transparency)
[forced-colors](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/forced-colors)
[inverted-colors](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/inverted-colors)
[light-level](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/light-level)

目前最新版的 Chrome (ver. 79) 已經有提供 `prefers-reduced-motion` 與 `prefers-color-scheme` 的 emulation，只要打開 Devtool，到 `Rendering` tab 下就能看到：

![devtool @media emulation](https://static.coderbridge.com/img/ArvinH/ec593dddc40d4030bbdb5a7f2bf99d14.png)

在演講中，講者 Adam 特別強調，`reduced-motion` 不是 `no-motion`，使用者想要少一點動畫，而非完全沒有動畫，可以從 演講中的範例感受一下（[ref](https://web.dev/next-gen-css-2019/#media-queries-level-5)、[codepen demo](https://codepen.io/argyleink/pen/RwwZMKm)）：

![reduced-motion](https://static.coderbridge.com/img/ArvinH/6ca565ec03eb439a9b66f5de287d062e.gif)

## logical properties

身為前端工程師，或多或少都會處理 i18n 的問題，在網頁排版上，不同語言間的 writing system 可能會造成我們需要個別為不同的語言客製化設定 css style，來調整 margin、padding 等等，而 Logical properties 就是希望能用更有效率、更好維護的方式來解決這問題。 [image ref](https://web.dev/next-gen-css-2019/#logical-properties)

這是我們熟知的 box model：

<img src="https://static.coderbridge.com/img/ArvinH/9d89a63601464607836370802e092ef5.png" style="max-width: 700px" />

Logical properties 將其改為：

<img src="https://static.coderbridge.com/img/ArvinH/78a8cb5f4113482a8f009f2400743435.png" style="max-width: 700px" />

差別在於從原本單純 `top`、`down`、`left`、`right` 外，多了 `block-*` 與 `inline-*` 兩個維度，根據使用語言的不同，瀏覽器會自動調整 `block-*` 與 `inline-*` 代表的屬性，例如在英文的寫作系統上，`block-start` 就代表 `top`，`inline-end` 代表 `right`。

有了 logical properties 的幫助，就能簡單的依據語言系統變更 `writing-mode` 與 `direction` 來調整 layout [ref](https://codepen.io/una/pen/mddxpaY)：

![logical layout example](https://static.coderbridge.com/img/ArvinH/47f5d3eaf6ba4545af05999e654f4700.gif)

## sticky situations

[`Position: sticky`](https://developer.mozilla.org/en-US/docs/Web/CSS/position#Sticky_positioning) 應該已經很多人用在產品當中了吧，但這次 Google 還是把它拿出來特別再介紹一番，提供了三種應用 sticky 的方式，蠻值得參考的：

![sticky-demo](https://static.coderbridge.com/img/ArvinH/1c9bfc06cbbc423eac2d68da391a3af3.gif)

仔細看每種應用的差別，基本上在於 sticky 的元素如何被”解除“ sticky，比較有趣的是 Sticky Desperado，利用 grid-system 達到 two column 的變化，可以到 codepen 上觀看實際程式碼：[Sticky Slide](https://codepen.io/argyleink/pen/YzzZyMx)、[Sticky Stack](https://codepen.io/argyleink/pen/abbJOjP)、[Sticky Desperado](https://codepen.io/argyleink/pen/qBBrbyx)

![caniuse-sticky](https://static.coderbridge.com/img/ArvinH/23c0b7fc70b540fc8632163d24e1c2c2.png)

## backdrop-filter

再來是我最喜歡的一個新屬性。以往我們要達到模糊圖片背景，並在上面加上文字的效果，可能需要這樣做：

<p class="codepen" data-height="382" data-theme-id="29194" data-default-tab="css,result" data-user="arvin0731" data-slug-hash="JjorEqG" style="height: 382px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="blur image with text">
  <span>See the Pen <a href="https://codepen.io/arvin0731/pen/JjorEqG">
  blur image with text</a> by Arvin (<a href="https://codepen.io/arvin0731">@arvin0731</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

需要把圖片設定成 `background-image`，設定 `filter: blur`，再透過 `position:fixed` 把文字釘上去。但有了 `backdrop-filter`，我們簡單將這個屬性套用在想要疊加的文字上，並直接放置於 `img` 元素：

<p class="codepen" data-height="362" data-theme-id="29194" data-default-tab="css,result" data-user="arvin0731" data-slug-hash="ExawZqj" style="height: 362px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="blur image with text-backdrop-filter">
  <span>See the Pen <a href="https://codepen.io/arvin0731/pen/ExawZqj">
  blur image with text-backdrop-filter</a> by Arvin (<a href="https://codepen.io/arvin0731">@arvin0731</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## :is()

`:is()` 這個僞類別已經存在很久了，但他們覺得很少人真正使用它，因此特別提出來介紹。
基本上 `:is()` 的功用就是能讓你將用逗號分隔的 selector，以參數形式放入其中，來達成同樣效果：

```js
button.focus,
button:focus {
  …
}
article > h1,
article > h2,
article > h3,{
  …
}

/* 與上面同樣效果 */
button:is(.focus, :focus) {
  …
}
article > :is(h1,h2,h3) {
  …
}
```

## gap

這邊指的 gap，就是 [CSS Grid Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout) 中的 gap，讓你不用為了製造出元素間的隔間，使用 margin，卻多出不必要的空隙 [img ref](https://web.dev/next-gen-css-2019/#gap)：

<img src="https://static.coderbridge.com/img/ArvinH/95dbce89c5b54d8db7b6497a7fbcec76.png" style="max-width: 700px" />

在演講中他們也有提到，`gap` 除了能用在 Grid layout 外，FireFox 也支援將其應用在 `flex` display 上頭，慣用 FireFox 瀏覽器的讀者可以嘗試看看。

## CSS Houdini、typed OM、Paint API、Worklet

CSS Houdini 是由一群來自 Mozilla, Apple, Opera, Microsoft, HP, Intel, IBM, Adobe 與 Google 的工程師所組成的工作小組，志在建立一系列的 API，讓開發者能夠介入瀏覽器的 CSS engine 運作，帶給開發者更多的解決方案，用來解決 CSS 長久以來的問題：

- Cross-Browser isse
- CSS Polyfill 的製作困難

CSS Houdini 提供的 API 讓你能存取 CSS Object Model，讓你透過 Javascript 延展 CSS 的功能，比起 CSS polyfill 能有更好的效能。

而 Worklet、Typed OM、Paint API 也都包含在 CSS Houdini 的規範中，在我之前的文章 [CSS 魔術師 Houdini API 介紹](https://blog.techbridge.cc/2017/05/23/css-houdini/) 都有詳細介紹過，有興趣可以前往細讀。

這邊我簡單說明 typed OM 與 Paint API 這兩個目前實作度較高的新功能，其中 Paint API 需要 Worklet 的輔助。

_typed OM_ 簡單來說就是就是 CSSOM 的強化版，最主要的功能在於將 CSSOM 所使用的字串值轉換成具有型別意義的 JavaScript 表示形態，例如你可以這樣操作 CSS style: (source from [CSS Houdini- the bridge between CSS, JavaScript and the browser](http://slides.com/malyw/houdini-codemotion#/27))

```js
// CSS -> JS
const map = document.querySelector(".example").styleMap;
console.log(map.get("font-size"));
// CSSSimpleLength {value: 12, type: "px", cssText: "12px"}

// JS -> JS
console.log(new CSSUnitValue(5, "px"));
// CSSUnitValue{value:5,unit:"px",type:"length",cssText:"5px"}

// JS -> CSS
// set style "transform: translate3d(0px, -72.0588%, 0px);"
elem.outputStyleMap.set(
  "transform",
  new CSSTransformValue([
    new CSSTranslation(0, new CSSSimpleLength(100 - currentPercent, "%"), 0),
  ])
);
```

用 `styleMap` 取得元素上以物件型態表示的 style 屬性，並能透過 `outputStyleMap` 來設定 style，其中還可看出多了 `CSSTransformValue` 與 `CSSTranslation` 這種 Class Interface。

而 _Paint API_ 則提供一個叫做 registerPaint 的方法：：

```js
registerPaint(
  "simpleRect",
  class {
    static get inputProperties() {
      return ["--rect-color"];
    }
    paint(ctx, size, properties) {
      // 依據 properties 改變顏色
      const color = properties.get("--rect-color");
      ctx.fillStyle = color.cssText;
      ctx.fillRect(0, 0, size.width, size.height);
    }
  }
);
```

宣告使用：

```css
.div-1 {
  --rect-color: red;
  width: 50px;
  height: 50px;
  background-image: paint(simpleRect);
}
.div-2 {
  --rect-color: yellow;
  width: 100px;
  height: 100px;
  background-size: 50% 50%;
  background-image: paint(simpleRect);
}
```

.div-1 與 .div-2 就可以擁有各自定義寬高顏色的方形 background-image。

不過這邊要注意一下，上面撰寫的 js 檔案，你可能會覺得就直接像一般 web 嵌入 js 的方式一樣即可，
但實際上並非如此，我們需要透過 _Worklets_ 來幫我們載入。以上面的 Paint API 為例：

```js
// add a Worklet
paintWorklet.addModule("simpleRect.js");

// WORKLET "simpleRect.js"
registerPaint(
  "simpleRect",
  class {
    static get inputProperties() {
      return ["--rect-color"];
    }
    paint(ctx, size, properties) {
      // 依據 properties 改變顏色
      const color = properties.get("--rect-color");
      ctx.fillStyle = color.cssText;
      ctx.fillRect(0, 0, size.width, size.height);
    }
  }
);
```

_Worklets_ 可以算是給 CSS Engines 使用的 worker，相對於 web worker 來說較為輕量、生命週期較短，適合用來處理 CSS engine 這種可能會牽扯到數百萬畫素圖片的工作。

實際範例可以參考 Google 提供的 [Codepen 範例](https://codepen.io/una/pen/VXzRxp?editors=1010)

## Others

還有許多新屬性在演講中沒有時間細講，就將其主要功能列在這邊，有興趣的人可以到 MDN 或 W3C 上搜尋。

- `size`: 可以讓你同時設定元素的寬、高屬性。
- `aspect-ratio`: 透過此屬性，再也不用利用 padding 等方式來製造等比例縮放效果了！
- `min()`, `max()`, `clamp()`: 這幾個函式提供你在各種 CSS 屬性加上數值的限制。
- `list-style-type`: 有新的 value 可以設置，像是 emoji 與 SVGs。
- `display: outer inner`: display 屬性之後能夠接受兩個參數，讓你明確的個別設置 outer 與 inner layout，而非使用 `inline-flex` 這種結合在一起的 keywords。

# 結論

CSS 的發展總是比較緩慢，畢竟需要各個瀏覽器實作配合，背後的因素可能不單單是純技術這麼簡單，但慢歸慢，還是能看得到前進的步伐，在新屬性普及前，就到 codepen 上弄個 side project 玩玩吧！
所有演講中提到的功能，只要是目前瀏覽器已經支援的，都有範例公布在他們的[範例網站](https://css-at-cds.netlify.com/)上，推薦前往試試！

## 資料來源

- [Vidoe - Next-generation web styling](https://youtu.be/-oyeaIirVC0)
- [Article - Next-generation web styling](https://web.dev/next-gen-css-2019)
- [Examples - Next-generation web styling](https://css-at-cds.netlify.com/)
- [你不知道的 CSS ： Next-generation web styling](https://juejin.im/post/5dd68a9be51d451e6d48c4b8)

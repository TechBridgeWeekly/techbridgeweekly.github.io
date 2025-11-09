---
title: GraphQL Summit 2019 與會分享
date: 2019-12-07 19:32:59
tags:
author: ArvinH
---

# 前言

記得剛上研究所的時候參加了第一屆的 WebConf，沒記錯的話是 WebConf Taiwan 2013。當時兩天的議程下來，接收到的知識與衝擊讓我精疲力盡，但心中是滿溢的富足感，自此之後，只要有機會我就會去參加各種 Conference。

然而或許隨著經驗增長，平常獲取知識的管道變多，能從演講者的演說內容吸取到的新知越來越少，對於參與會議變得興致缺缺，直到我理解到在會議現場與人互動交流其實才是 Conference 的一個主要功能後，我才又再度燃起動力。

剛好目前所待的公司，對於補助員工前往國外參與會議非常大方，從加入後我就一直搜尋有什麼想參與的會議，但籤運一向不好的我，果不其然沒抽到辦在 Las Vegas 的 React Conf 2019; 今年的 Chrome Dev Summit 也沒被選中; 更錯過 React Finland 跟 React Rally 的最後幾張票。

然而在接近年尾的十月底，Team 上同事大力推薦 GraphQL Summit，剛好當時手上有個專案使用 Apollo 作為主要框架，對於 GraphQL 處於熟悉與不熟悉之間， 抱持著『以這種狀態去參加會議，或許就能再次體驗到當年參與 WebConf 的那種新知衝擊』的想法，就一起去了，也的確收穫不少。

回來到現在其實過了一個多月，趁著還沒有完全忘光，趕快記錄下來與大家分享，包含我有聽的議程，以及會議期間的一些見聞。

# GraphQL Summit

GraphQL Summit 是目前專門以 GraphQL 為主題的最大會議，從 2015 年 GraphQL 開源後，連續舉辦了 GraphQL Summit 2016、2017、2018 與 2019，主要由 Apollo 所主辦，往年 Facebook、Twitter、GitHub 等大公司也都有贊助或演講，今年的贊助商名氣不如以往（我本來心心念念想搜刮 GitHub 貼紙...），場地卻升級不少，舉辦在舊金山的凱悅飯店。

![GraphQL Summit 2019 Entry](https://static.coderbridge.com/img/ArvinH/346dbd0cbfe047caa5f1901e9814a963.png)

今年的會議分為三軌，除了一般議程外，中間也有一軌會穿插 Lighting talks，主題橫跨前端與後端在 GraphQL 上的應用、Best practice、工具與實戰（踩雷）經驗分享，除此之外，中午時間在餐廳區域也有畫分出各種主題的 topic table，有 Apollo 的員工作為主持人，引導大家針對該桌的主題進行討論，邊吃午餐邊進行技術討論的氛圍很不錯。

至於議程部分，其實大多是聚焦在 Server 端的主題，比例上來說前端議程偏少，而我主要還是挑選與前端相關的議程參與，以及一些 Apollo 推廣自家框架的 Talks。

接下來我會從我有聽的議程中，擷取我覺得比較有趣的演講，做些簡單地整理介紹，並附上我有收集到的講者投影片與影片連結，如果看完覺得有興趣，可以到官方的 Youtube 網站上觀看完整演講，目前 [GraphQL Summit 2019 的影片](https://www.youtube.com/playlist?list=PLpi1lPB6opQyraZSmwFre_FpL00_3nTzV)都已全數上架！

## 在進入議程內容前...

先跟大家分享一些會議中的小趣事充充版面 :p

這張照片是第一天早上排隊註冊時拍下的，可以看到主辦方用 `Last Name` 作為分流的依據，但人潮很明顯還是集中在一排上，畢竟，即使有來自世界各地的與會者，姓氏要能均勻分布在各種字母上的機率還是不高啊。雖然照片中排隊人數還不多，但我在那邊觀察了一段時間，不少外國人也都發現這個現象，議論紛紛 XD

![lineup](https://static.coderbridge.com/img/ArvinH/4510106525044fecbeb0d2d45d3e1f57.png)

另外一個我覺得莫名其妙但好感度很高的是 puppy booth 🐶

![puppy booth](https://static.coderbridge.com/img/ArvinH/900548092e054f2eb09497b525d187e2.png)

完全不知道為什麼要弄一個小圍欄然後大家可以進去玩狗狗 XD 真的很療癒就是了。

![apolloween](https://static.coderbridge.com/img/ArvinH/9ecb40321f664dfda2a58835d1b200f2.png)

會議期間恰逢萬聖節，第一天議程結束後的晚餐順便辦了 apolloween party，請了個 DJ 來放音樂，有些會眾也自帶服裝來替換，可惜工程師就是工程師，最嗨的總是主辦方自己 XD

這種會後 party 就是喝酒尬聊的場合，比較有印象的是遇到一個在 CodeSandbox 工作的大哥，隨身攜帶公司的 stickers，大概是交朋友的利器，當然我也拿了不少；身為 CodeSandbox 使用者，跟他~~提了不少需求，一起抱怨 TypeScript~~ 聊了不少，整體還算有趣。

閒話說到這邊，來進入正題。

# Keynotes

## [Opening Keynote - Apollo Co-Founder & CTO](https://youtu.be/EDqw-sGVq3k)

負責 Kicks off GraphQL Summit 的是 Apollo 的 Co-Founder 兼 CTO, Matt Debergalis。基本上就是來介紹他們家產品，並帶出後續議程的大略方向。

這場 Keynote 中，他重述了 GraphQL 在整體專案架構中所能帶來的好處，將以往後端與前端 M to N 的溝通模式，改為中間統一以一層 **Data Graph** 集中資料並映射到不同前後端的新架構，而 GraphQL 就是實作該 Data Graph 的最好選擇。包含 NY Times、Paypal 等公司都投入大量資源在 GraphQL 上，Paypal 內部有五十多個產品都是採用 GraphQL 技術作為前後端資料溝通的媒介。

![data-graph](https://static.coderbridge.com/img/ArvinH/e23ed749dead4c7c9df61bda35671b18.png)

此外，他也推薦大家去閱讀他們所編寫的 [Principled GraphQL](https://principledgraphql.com/)，根據上百間公司導入 GraphQL 的經驗總結出一套準則，並分類為三大塊來探討：_Integrity_、_Agility_ 與 _Operations_。
**Integrity** 聚焦在 **One Graph** 的概念，每個 Team 應該共享一個統一的 Graph，但實作上還是每個 Team 各自負責。
**Agility** 則是提醒大家，要能夠採用敏捷開發的準則來實作，Graph 的 Schema 要能夠根據實際需求的變化來增長或改變。
**Operations** 著重在如何將 Graph 安全地部署到你的線上環境，包含如何 Logging 與 Separate concerns。

Apollo 推出的 [Federation](https://www.apollographql.com/docs/apollo-server/federation/introduction/) 與 [Graph Manager](https://www.apollographql.com/docs/graph-manager/) 就是幫忙實現這些準則的工具。今年也有個議程在介紹 [Federation Architecture](https://youtu.be/LKQKn1oFXJU)。

前端部分，他介紹 Apollo、React Hooks 與 TypeScript 之間是如何完美匹配，搭配 codegen，自動依據 GraphQL Query 產生對應的 Type Interface，再交由 VS Code 等編輯器來啟用 Auto Complete 等功能。

接著以 Search 為例，引出 Apollo Client 3.0 在處理前端 Cache（Components、Local State、Network transport） 上所提出的 Unified Cache Model，讓 Apollo Client 幫你處理常見 Pattern 的 Cache 問題，並提供簡易的 API 讓你在像 Search 這類需要不同 Cache 策略的功能上方便調適。

Keynote 的總結是，**GraphQL is mature and approachable**

![keynote summary](https://static.coderbridge.com/img/ArvinH/f6ecba278a4946e2a3f896d13b735e05.png)

## [Migrating to GraphQL at Airbnb (Brie Bunge, Software Engineer at Airbnb)](https://youtu.be/pywcFELoU8E)

這場也算是 Keynote，是我兩天議程下來第二喜歡的一場 Talk（第一喜歡的等等就會介紹到），推薦大家到 Youtube 上觀看，也就 20 分鐘左右。由任職於 Airbnb 的 Brie Bunge 分享他們內部如何從 react/redux/rest api 的架構轉移到 react/apollo/typescript 的過程。

去年的 GraphQL Summit 他們就已經分享過 GraphQL 如何大幅提升他們的開發效率，而一年來他們將 GraphQL 移植到越來越多的 team 內，像是 PWA 版本的 Mobile Web。

在 Migration 的策略上，Airbnb 採用 **Incremental adoption**，他們將整個 Migration 的過程分為五個階段，每個階段的目標都是要能產生一個 shippable 且 fully funcational 的產品。

在執行 Incremental adoption 前，有些 pre-requisites 他們需要先滿足：1. 在後端使用 GraphQL（可以看他們的 [Medium 文章](http://bit.ly/36jy2Xd)）；2. 採用 TypeScript，利用 codegen 從 GraphQL Schema 產生與 Query 對應的 Type Interface，保持前後端的 Single source of truth。TypeScript 也成了 Airbnb 內部的官方前端語言（可以看同個講者在 [JSConf 的演講](https://youtu.be/P-J9Eg7hJwE)）。

Incremental adoption 分為五個階段：

1. Rest -> GraphQL
   - 這個階段的目標是要確保前後端的整合是沒問題的，也就是得確保 GraphQL Schema 與原先 Rest API 提供的資料能夠 Match，做法上前端可能得在 Query 前實作一些 Adapter 來修改 request params 與 response data。
   - 另外也必須要能夠依據 Schema 產生 Typescript Type。光是 HomeCheckoutFlow 這個 feature 就能產生超過一千行的 auto-generated TypeScript Types。
   - 在這個階段他們不更新 React Component，也不修改 API Response Shape，因此可能會有 Over-fetching 的問題，但至少完成此階段時，他們還是保有 Shippable 的產品。
2. Propagate Types
   - 第二階段目的在於加強 TypeScript Types，來提高後續階段的實作信心。在此階段不碰觸任何 Runtime behavior 與產品行為。
   - 利用 Generated types 來取代任何程式碼內的 `any` Types。
   - 有了完整的 Types 後，常常會需要進行 deep properties check，像是 `data && data.reservations && data.reservations[0]`。他們利用 [IDX](https://www.npmjs.com/package/idx) 這個套件來作為 optional chaining 出現前的暫解。
3. Apollo HOC -> Hooks
   - HOC 與 Hooks 現在是共存在 code base 中，但積極的從 HOC 移植到 Hooks 上。（`useQuery`, `useMutation`）
   - 從 Redux Store 移植到 Apollo Cache，但在此階段他們還未把 Redux Store 完全拿掉，因為有些資訊並沒有存到 Apollo Cache 中，但其扮演的角色已經不那麼重要了。
4. Granular Query Fragements
   - 在這個階段，Airbnb 的 Migration 尚有 Over-fetching 問題要解決。以下圖為例，他們從 Component Tree 的最下層開始，也就是最先開始 render 的 Component 開始採用 Query Fragements，讓每個 Container 自己決定要拉取哪些資料，並共享 Fragements，逐步解決 Over-fetching issue。![Granular Query Fragements](https://static.coderbridge.com/img/ArvinH/ec630598b2df4c4988807b5d3d3c0811.png)
5. State Management
   - 當所有 React Component 都移植到了 Apollo 上後，他們開始檢視剩餘的 Redux state management。他們發現所有僅存的 Redux Store 所儲存的狀態都能用 React State、Context 與 Apollo cache + resolvers 取代。
   - 如此一來就能在整個 State management 上有一致的 mental model，並且減少 Redux 帶來的 boilerplate，還能享用 Apollo 自帶的 Caching 功能。

演講最後她提到了目前團隊正在研究的方向，其中的 Service Worker Query Pre-fetching 很有意思，她以 Server-side rendering 為例，提到可以將 GraphQL Query 從 Component level 拉到 route level，如此一來就能透過 Service Worker 提早觸發 GraphQL Query request，大量縮短使用者從看到 Server-side rendered 的 HTML 與頁面實際能操作的時間差。

![SW query pre-fetching](https://static.coderbridge.com/img/ArvinH/d40a7587c09648db8d25cec342b48e53.png)

# Talks

接下來我想先介紹兩場我印象最深刻的 Talks，以免大家因為內容太長而錯過，這兩場 Talks 除了內容扎實外，各自有獨特之處值得一提。

## [useSubscription: A GraphQL Game Show (ALEX BANKS - Software Engineering Instructor at Moon Highway)](https://youtu.be/QUeL-GfNJVU)

Alex Banks 在這場演講中儼然是個節目主持人，唱作俱佳，時間掌控也超好，以充滿互動式的演講帶出 Subscription 的用法與實作上的要點。我在之前從沒使用過 SubScription，但看完他的 Talk 後，對整體運作與如何使用有了非常清晰的輪廓，極力推薦大家點進影片觀看整場 Talk，看他如何現場用 Mutation 與 SubScription 切換音樂，又如何以 "Live Coding" 的方式說明 Client 端需要事先 Setup 哪些東西才能讓前後端的 SubScription 串接起來。

![GraphQL Game Show](https://static.coderbridge.com/img/ArvinH/6fe6c13f268744e3a0c9452f22fe72cb.png)

實際上 SubScription 就是開啟一個 WebSocket 連線來 PubSub 資料 。透過 SubScription，能輕易實作出互動遊戲，例如他在現場用 Mutation trigger 一個 Question，現場聽眾只要連進他的伺服器就會接收到問題，並且進行回答， 使用者透過另一個 Mutation 將答案推回 Server，更新狀態後，SubScription 再推回給所有 Client 端。

影片中他利用 `ApolloLink` Interface 中提供的 `Split` 函式來區分進來的 Query 要使用一般的 HttpLink 或是需要 SubScription 處理。

另外演講的最後，他提到 Apollo 提供的 PubSub 是運行在你 Nodejs 的單ㄧ Instance 上的，所以當你的 service 需要 scale 時，就必須要注意，可能採用 Redis 會是比較好的替代選擇。

非常有趣的一場演講，節奏流暢，說明清晰，也為他與他老婆創辦的程式教育平台 Moon Highway 打了最好廣告，他也有出版一些書籍：[Learning GraphQL](http://shop.oreilly.com/product/0636920137269.do)、[Learning React](http://shop.oreilly.com/product/0636920049579.do)。

## [GraphQL Search (Artem Shtatnov, Software Engineer at Netflix)](https://youtu.be/04l8eLrGNSw)

這場 Talk 我是衝著講者來自 Netflix 去聽的，實際上的內容因為沒有接觸太多 Search 功能，所以聽起來感受不深。會想特別提這場演講是因為，講者本身應該是有[妥瑞症](https://zh.wikipedia.org/zh-hant/%E5%A6%A5%E7%91%9E%E7%97%87)，想當然爾，演講的過程就不如他人順暢，從自我介紹開始，大家就注意到了，當時我特意觀察了在場的聽眾，蠻訝異的是還是有少數人露出些許不耐的表情，但講者很認真的講完整場，而且其實內容很清楚，結束後的掌聲比起前面幾位講者都大了很多。

簡單介紹一下演講內容，是介紹 Netflix 如何根據 data graph 裡存在的 graph edges 來建立 Search Indices，進而在整個 Graph 上進行有效搜尋。

主要利用 Kafka 來處理 change event，告知 Indexer 有改變發生；GraphQL 針對 change 改變資料；Elasticsearch 作為 Search database 儲存資料。這三者組合成他們的 Search Indexer。

你可以在他們的[官方 Blog 上找到文字版的說明](https://medium.com/netflix-techblog/graphql-search-indexing-334c92e0d8d5)。

另外附上講者的網站：[artemshtatnov.com](https://artemshtatnov.com)

## [Components as Data: A Cross Platform GraphQL Powered Component API (Luke Herrington, Senior Javascript Engineer at Four Kitchens)](https://youtu.be/k7oOShYz0R0)

這位講者在我們第一天午餐時，剛好跟我們做同一桌，聊天過程中得知他在做的東西，跟我手上目前的一個專案做的事情簡直一模一樣，所以就去聽了他的演講。

基本上他的想法是，讓前端頁面的 Layout 與 Component 組成資訊交由後端來儲存與決定，前端只需要 Mapping 真正的 Component 程式碼，這樣一來 PM 想要調整 Layout，或是需要 A/B Testing、設置 Feature Flag 時，只需要更動後端資料，前段就能自動反應。

這種作法在我手上的專案很適合，因為使用情境的切合。但對我們來說，因為不是每個頁面都會用到所有的 Component，若是前端需要存放一個包含所有 Component 的 Map，會蠻浪費的，因此需要 Dynamic loading，而 Dynamic loading 到了 SSR 上又是另一個需要處理的麻煩事務。

演講結束後我有去向他詢問類似問題，想知道他是如何解決前端需要儲存一個 Component Map，但可能不是每個頁面都會用到所有 Component，這種資源浪費的問題，但果然以他的 Use Case 來說，並不太需要考慮到這問題。但即便如此，看到自己平常實作上用到的架構被包裝成一個演講的 Topic 感覺還是蠻有趣。

## [Optimistic UI: Predicting the Future (KENNY HAMMERLUND)](https://youtu.be/465bHdrcU7s)

這場是 Lighting talk，被名字吸引而來，後來發現其實他做的事情很簡單，而且大部分前端應該都是這麼做的。

想像一下你在製作一個留言版，當使用者編輯或是新增留言時，你會送出 Request 後，重新再從 Server 端拉一次資料，還是你會將 Local state 直接更改呢？相信多數人會採用前者做法。

這場演講就是在講這件事，只是說，Apollo Client 讓這件事變得非常容易，它提供的 Optimistic Function，讓你在 Query 中指定該如何在 Server 資料還未 Response 時，先行更新資料。

![Optimistic UI](https://static.coderbridge.com/img/ArvinH/900576e4c146493d9880364e77a68164.png)

也可以參考 Apollo 官方網站上的文章：[Optimistic UI](https://www.apollographql.com/docs/react/performance/optimistic-ui/)

## [A Treatise on State (Jed Watson, Partner at Thinkmill)](https://youtu.be/tBz3UmZG_bk)

這場 Talk 跟 GraphQL 關係不大，但我覺得蠻有意思的，講者探討我們在進行 State management 的同時，應該要先針對 State 進行分類，根據不同的分類用不同的方式管理。

![Treatise on State](https://static.coderbridge.com/img/ArvinH/cc4b1461ac264652ace278b34c812e6a.png)

講者將 State 分類為五類：

1. Local State
   - Component State（元件內的狀態）
   - Form State（表單內的狀態）
   - UI State（其他 UI 操作更動的狀態）
   - 最基本的狀態，通常會以 Props 方式傳遞給其他 Component
2. Shared State
   - App State
   - Data (cache or store)
   - UI State (Shared concerns)
   - 只有在你知道你需要 Shared state 時，才需要來存取
   - Local state 要變成 Shared State 的先決條件在於，兩個或以上的元件需要存取、操作同一塊 State。不過要注意 Props drilling，該以 Context 方式進行共享。
3. Remote State
   - 你不擁有的狀態
   - 你可能會需要 Cache 它，或是從該狀態取得另外的值
   - Local changes 會透過 API 來與其進行 Sync
   - 該狀態永遠是 async 且不可靠的
   - 講者沒有給很明確的例子，但依我理解，指的是 Server 端提供的資料狀態。
4. Meta State
   - 提供給你參考的狀態，你不是狀態的來源
   - 有 API 會改變其狀態
   - Always respect it
   - 不是 Remote State 的一部分，只是常常一起出現
   - 舉例來說，input box 的 Auto complete 功能，當你 Typing 時，等待 response 時的 Loading state。
5. Router State
   - 提供給你參考的狀態，你不是狀態的來源
   - 有 API 會改變其狀態
   - 不是 Remote State 的一部分，只是常常一起出現
   - 準則與 meta state 相同，主要指的是當你網站的 Route 改變時，你通常會因應而呈現不同的 Component，這就是 Router State。

講者提到幾點從 Redux 轉換到 Apollo 來管理 State 時的好處，基本上減少了大量的程式碼，並提高了 codebase 間的透明度：

- 透過 Apollo，你可以只選擇你想要的 Data，拿到資料後也不需要額外的轉換函式來處理資料。
- 資料的 Request 與 Component 是放在一起的。
- Meta State 直接由 Apollo 自動幫你處理好。

> Don't solve problems when you can stop having them

演講最後的總結是建議大家建立完整封裝的 Component，每個 Component 管理自己的 State，然後組合這些 Component 來完成複雜的 UI。不要試圖選擇單一框架來管理 State，應該依據不同類型的 Type 來選擇不同的框架處理。只要你越了解每個狀態的區別，越知道該如何用不同的策略管理他們，那麼用什麼工具來管理狀態就愈不重要。

## [How do you get changes made to GraphQL? (Orta, Engineer On Typescript at Microsoft)](https://youtu.be/STk8eWQL1ns)

這場演講的內容與其他很不相同，講者本身資歷豐富不說外，別的講者都是在說怎麼使用 GraphQL，但這場是在介紹 GraphQL Fundation 的歷史，以及如何參與 GraphQL Specs 或是其他 first-party projects 的提案修改。從 GraphQL 的歷史簡短介紹開始，到如何提案，與其過程為何。

其中講者有提到 Youtube 上的一個紀錄片，[GraphQL: The Documentary](https://youtu.be/783ccP__No8)，半小時的內容，介紹了 GraphQL 的來龍去脈，推推！

總之在一連串的歷史發展與各大公司間的合作後，有了現在的 GraphQL Fundation，負責統籌與發展 GraphQL Ecosystem。他們有維護一份 GraphQL Landscape，讓你能宏觀的了解整個 Ecosystem 的狀態：

![GraphQL Landscape](https://static.coderbridge.com/img/ArvinH/6c0c59fa978b4704a2f7ec0f2b9e2486.png)

此外，GraphQL Fundation 的會員是需要付會員費的，等同於維護這個 Fundation 的人是 get paid 的，而會員有權決定組織走向與預算開銷。

GraphQL.org 主要負責幾項事物，包含 Spec、Reference Implementations 與 GraphiQL 以及 Branding 和剛剛提到的 Landscape 等等。

此外有兩個 Working Group：GraphQL Working Group & GraphiQL Working Group。每個 Working Group 都有 Agendas，都放在 github 上頭，可以 follow 了解最新發展。

想要參與 Working Group，他們有一些 guideline 需要遵守：

![participation guidelines](https://static.coderbridge.com/img/ArvinH/68aa5acb679c43a397c0802178fb3e74.png)

要針對 GraphQL Spec 進行更動，有幾個階段需要執行：

1. Strawperson - 這階段指的是你可能在 GitHub 上開了 Issue，接受大家各種意見的"衝擊"，進而反覆修改你的想法。
2. Proposal - 這階段是你可以開始思考發 Pull Request，無論是修改 Spec 或是 Reference Implementation，接受大家不同面相的意見。
3. Draft - 用正確的 Syntax 與 Format 去更改 Spec 或 Implemenaton。
4. Accepted - 最後階段就是你的提案被 Accepted，然後在下一次的 Release 中被釋出。

不過你的提案當然也有可能被 Rejected，這時可以參考 GraphQL 的 Guiding Principles，找出問題癥結：

![Guiding Principles](https://static.coderbridge.com/img/ArvinH/5d8446a8aca14a5e9b2373b7dea6c612.png)

上述是更動所需要的步驟，而如何參與的步驟如下：

1. Have an Idea
2. Join a working group
3. Pitch
4. Start doing work

步驟簡單，但就如同一般參與 Open Source project 一樣，你的 PR 可能很久都不會被 Merge 進去，需要有足夠毅力，並認知到每個人的時間都很有限，不要給自己太多壓力。

# Other Talks

在我參與的議程中，還有兩個我覺得內容蠻不錯值得一看的是：[Game Of Types: A Song Of GraphQL And TypeScript (Steven Musumeche, Senior Software Engineer at Formidable)](https://youtu.be/AgJ1n75ibCo) 與 [How We Scaled GraphQL at The New York Times (James Lawrie, Lead Software Engineer at NYT)](https://youtu.be/gpd6JtnWs2E)。

礙於時間與篇幅，我暫時先寫到這邊，後續若有時間會再來補上這兩篇的重點分享，有興趣的讀者推薦到影片連結去觀看。

## 結論

GraphQL Summit 整體來說是非常棒的會議，內容豐富外，動線規劃與議程中的 Topic tables 與 Sponsor Booth 都規劃得不錯，即使該時段沒有有興趣的 Talk，場外也很多贊助商或是 Apollo 的員工可以跟你討論聊天。美中不足的是第二天有些場地不知為何突然限制人數，連在場內站著聽都無法，所以我錯過了一些場次，但好在他們都有上傳影片，他們甚至因為有一場演講沒有錄到影，把講者再請回來重新演講一次。

從今年的 GraphQL Summit 來看，GraphQL 的發展已經頗成熟，Apollo 建造的 Ecosystem 也很友善，之後我應該也會盡量找機會將其採用至其他專案中！若你還沒導入 GraphQL 到專案中的不仿試試，可以到台灣的 GraphQL 社群 - https://www.facebook.com/groups/graphql.tw/ 提問討論。

最後附上幾張加州景色作為結束：

![castrostreet](https://static.coderbridge.com/img/ArvinH/cbacf86ea1f84396ba84d66a7b2aaf88.png)

![goldengate](https://static.coderbridge.com/img/ArvinH/333fa4ecfac244b2a80dc230c9968c90.png)

關於作者：
@arvinh 前端攻城獅，熱愛數據分析和資訊視覺化

title: 渐进式 Web 应用（PWA）
speaker: Joel
prismTheme: dark
plugins:
    - mermaid

<slide class="bg-black-blue aligncenter" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::{.content-right}
# PWA - 渐进式Web应用 {.text-shadow}

by Congyu Shen

[:fa-github: Github](https://shenjoel.github.io){.button.ghost}
[:fa-cloud-download: Demo](https://shenjoel.github.io/){.button.animated.fadeInUp}
:::
:::note
<p style="text-shadow: none; color: black">1、为什么会出现 PWA</p>
<p style="text-shadow: none; color: black">1.1、回顾一下历史，在 2015 年之前的那段时间，作为前端开发人员，我们主要精力花在哪里，对于我来说，移动站点的性能优化是投入精力很大的一部分，例如提升首屏速度，动画的流畅度，经过一段时间的优化，性能确实有不小的提升，但是不管怎么优化，还是比 Native App 要的体验差很多，始终无法突破移动设备上 WebView 给 Web 的枷锁，这就是我们想说的第一个问题，Web 的用户体验。</p>
<p style="text-shadow: none; color: black">1.2、除开用户体验问题之外，还有一个非常重要的问题，那就是用户留存。Native App 安装完毕后会在用户手机桌面上有一个入口，让用户打开 App 只需一次点击，而 Web App 在移动时代最主要的入口还是搜索引擎，用户从浏览器到站点需要经过搜索引擎，如果想访问上次同样的内容甚至还需要记住上次的搜索词，用户也可以记住 URL 并进行输入，但这些对于移动用户来说，无疑成本巨大，这就导致 Web 站点和用户之间的粘性非常脆弱。Native App 还能够通过发送通知让用户再次回到应用中来，而 Web 没有这个能力。</p>
<p style="text-shadow: none; color: black">1.3、Device API 的不完善。Android 和 iOS 提供了非常丰富的设备 API，Native App 只需获取用户授权就可以使用，而在 Web App 中，WebView 没有提供这样的 API，完全没法使用，如果我们开发一个需要使用 NFC 的 App，你一定不会考虑 Web，因为近场通信 API 在 Web 中还没有。虽然在近年来，W3C 已经提出了很多新的标准，但是浏览器对于 Device API 的支持仍然很不完善。</p>
<p style="text-shadow: none; color: black">2、在开始之前，我们先来看看一个效果视频</p>
:::

<slide class="bg-black-blue aligncenter" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::{.content-right}
<video id="localImportVideo" playsinline="" controls="" disablepictureinpicture="" muted src="https://cdn-files.myshoplinestg.com/video/store/4200012610/1625907878655/show-pwa.mp4?w=800&amp;h=1280&amp;d=32.833333" width="100%" height="800" preload="auto"></video>
:::

<slide class="bg-black-blue aligncenter" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
!![](./img/webVSna_uv.png)
 #### Web站点每个月的UV是Native App的`3 倍`，然而用户在Native App花费的时间却是Web的`20 倍`{.text-intro}

:::note
<p style="text-shadow: none; color: black">Google 在一篇名为《Why Build Progressive Web Apps》的文章中披露过这样的一组数据，Web 站点每个月的 UV 是 Native App 的 3 倍，然而用户在 Native App 花费的时间却是 Web 的 20 倍，如下图所示，这之间巨大的反差，和上面所说的三个原因息息相关。</p>
:::


<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::{.content-right}
## 什么是PWA{.text-landing}
- Web App Manifest
- Service Worker
- Web Push
{.build.moveIn}
:::
:::note
<p style="text-shadow: none; color: black">Google 提出 PWA 的时候，并没有给它一个准确的定义，经过这么多年的实践和总结， PWA 它不是特指某一项技术，而是应用多项技术来改善用户体验的 Web App，其核心技术包括 Web App Manifest, Service Worker, Web Push 等，而这些技术都是围绕着提升用户体验进行，这就是 PWA 的核心。</p>
:::


<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### 添加到主屏幕 - 配置
:::
:::{.content-right}
### Manifest\:{.content-left}
```json
{
  "name": "Joel's blog",                --> 应用名
  "short_name": "Blog",                 --> 应用名缩写
  "icons": [{                           --> 展示在设备屏幕在图标
      "src": "./img/192x192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "maskable"
    }, 
    {
      "src": "./img/512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "start_url": "/index.html",           --> 打开的链接
  "background_color": "#ffffff",        --> 启动的背景色
  "display": "standalone",              --> 展示方式
  "theme_color": "#2874f0"              --> 主题色
  ...
}
```
`<link rel="manifest" href="./manifest.json" />`
{.build.moveIn}
:::
:::note
<p style="text-shadow: none; color: black">添加到主屏幕是现代浏览器中的一项功能，在chrome（等一些现代浏览器）中，你可以将访问的网站添加到桌面，这样就会在桌面生成一个类似“快捷方式”的图标，当你点击该图标时，便可以快速访问网站（Web App）。而刚才提到的 Web App Manifest，是一份web app的清单，用来配置添加主屏的入口文件，存放在资源要目录中。</p>
:::


<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### 添加到主屏幕 - 安装
:::

:::gallery-overlay{..bg-light}
!![](./img/a2hs-install-01.jpeg .size-90)
## 
---
!![](./img/a2hs-install-02.jpeg .size-90)
## 

---
!![](./img/a2hs-install-03.jpeg .size-90)
## 

---
!![](./img/a2hs-install-04.jpeg .size-90)
## 
:::

:::note
<p style="text-shadow: none; color: black">配置好之后，我们可以通过设置添加到主屏安装选项，点击之后会有一个安装提示，确认后主屏幕上会新增一个快捷方式，也是当前web app的入口，实际的开发中自己去实现安装的UI，都是可以自定画的，那么到这里主屏添加的功能就已经实现了。点击当前这个快捷入口，它会像原生app一样在独立的容器打开，有一个丝滑的入场动效，并且打之后界面几乎和原生应用一样，对于web站点来说，这算是一个跨越性的尝试</p>
:::


<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::{.content-right}
### 添加到主屏幕 - 特点
- Web App可以被添加到桌面并有它自己的应用图标；
- 从桌面开启时，会和原生app一样有它自己的“开屏图”；
- 更进一步的，这个Web App“外套”几乎和原生应用一样，没有浏览器的地址栏、工具条，和Native App一样运行在一个独立的容器中。
{.build.moveIn}
:::

:::note
<p style="text-shadow: none; color: black"></p>
:::

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### 添加到主屏幕 - [:fa-link:](https://shenjoel.github.io/)清单检查{.text-intor}
:::
!![](./img/a2hs-install-checklist.png .size-80.alignleft)
:::note
<p style="text-shadow: none; color: black">清单配置项可以通过Chrome控制台查看是否正确</p>
:::


<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### Manifest 属性值兼容性
:::
<div style="width: 1200px; height: 700px; overflow: auto;">
	<img src="./img/Web-app-manifests-MDN.png" />
</div>
:::note
<p style="text-shadow: none; color: black">不同的系统和浏览器的兼容情况也是大不相同，移动端Android Chrome的支持是最完整的，在桌面端Chrome和Edge也是支持比较好的，当然在移动端的体验是最好，甚至提供了与原生APP一至的在，打开应用时的入场动效，单独的浏览容器，那我们来看下实际的交互场景是怎样的（转Demo）</p>
:::

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::{.content-right}

##### PWA其中一个令人着迷的能力就是离线（offline）可用，离线只是它的一种功能表现而已，具体说来，它可以：
---

* 让我们的Web App在无网（offline）情况下可以访问，甚至使用部分功能，而不是展示“无网络连接”的错误页； {.animated.fadeInUp}
* 让我们在弱网的情况下，能使用缓存快速访问我们的应用，提升体验；{.animated.fadeInUp.delay-400}
* 在正常的网络情况下，也可以通过各种自发控制的缓存方式来节省部分请求带宽； {.animated.fadeInUp.delay-800}

:::{.build}
###### `而这一切，其实都要归功于PWA背后的英雄 —— Service Worker。`
:::

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### Service Worker
:::
:::shadowbox

## 代理层
Service Worker 是浏览器和网络之间的虚拟代理

---

## 独立工作线程
不同于页面主线程，独立运行在另一个工作线程上，它无权访问 DOM 结构
:::

```mermaid
graph TD
	UA --> service_worker
	service_worker --> UA
	service_worker --> Server
	Server --> service_worker
```

:::note
<p style="text-shadow: none; color: black">Service Worker 是浏览器和网络之间的虚拟代理</p>

<p style="text-shadow: none; color: black">不同于页面主线程，独立运行在后台的另一个工作线程上</p>

<p style="text-shadow: none; color: black">早在 2014 年 5 月 W3C 就提出了 Service Worker 草案，用来进行 Web 资源和请求的持久离线缓存。Service Worker 的来历可以从两个方面来介绍。</p>
:::


<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### Service Worker
:::
:::shadowbox

## 释放主线程的性能压力
将复杂的耗时的工作脱离主线程执行，释放在主线程的性能耗时。

---

## 持久离线缓存能力
可以通过自身的生命周期特性保证复杂的工作只处理一次，并持久缓存处理结果，直到修改了 Service Worker 的内在的处理逻辑。
:::

:::note
<p style="text-shadow: none; color: black">一方面，浏览器中的 JavaScript 是运行在一个单一主线程上的，在同一时间内只能做一件事情。随着 Web 业务不断复杂，在 JavaScript 中的代码逻辑中往往会出现很多耗资源、耗时间的复杂运算过程。这些过程导致的性能问题在 Web App 日益增长的复杂化过程中更加凸显出来。所以 W3C 提出了 Web Worker API 来专门解放主线程，Web Worker 是脱离在主线程之外的工作线程，开发者可以将一些复杂的耗时的工作放在 Web Worker 中进行，工作完成后通过 postMessage 告诉主线程工作的结果，而主线程通过 onmessage 得到 Web Worker 的结果反馈，从而释放了主线程的性能压力。</p>

<p style="text-shadow: none; color: black">代码执行性能问题好像是解决了，但 Web Worker 是临时存在的，每次做的事情的结果不能被持久存下来，如果下次访问 Web App 同样的复杂工作还是需要被 Web Worker 重新处理一遍，这同样是一件消耗资源的事情，只不过不是在主线程消耗罢了。那能不能有一个 Worker 线程是一直可以持久存在的，并且随时准备接受主线程的命令呢？基于这样的需求 W3C 推出了最初版本的 Service Worker，Service Worker 在 Web Worker 的基础上加上了持久离线缓存能力，可以通过自身的生命周期特性保证复杂的工作只处理一次，并持久缓存处理结果，直到修改了 Service Worker 的内在的处理逻辑。</p>

<p style="text-shadow: none; color: black">让我们简单了解一下 Service Worker 生命周期的工作原理</p>
:::


<slide class="fullscreen bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### Service Worker - lifecycle
:::

:::column
!![](./img/sw-lifecycle.png)

---

:::{.fadeInRight}
- 1、在主线程成功注册 Service Worker 之后，开始下载并解析执行 Service Worker 文件，执行过程中开始安装 Service Worker，在此过程中会触发 worker 线程的 install 事件。{.bg-primary}

- 2、如果 install 事件回调成功执行（在 install 回调中通常会做一些缓存读写的工作，可能会存在失败的情况），则开始激活 Service Worker，在此过程中会触发 worker 线程的 - activate 事件，如果 install 事件回调执行失败，则生命周期进入 Error 终结状态，终止生命周期。{.bg-primary}

- 3、完成激活之后，Service Worker 就能够控制作用域下的页面的资源请求，可以监听 fetch 事件。{.bg-primary}

- 4、如果在激活后 Service Worker 被 unregister 或者有新的 Service Worker 版本更新，则当前 Service Worker 生命周期完结，进入 Terminated 终结状态。{.bg-primary}
{.build}
:::

:::

:::{.build.fadeInRight.aligncenter}
#### `通过 Service Worker 让 PWA...?`
:::

:::note
<p style="text-shadow: none; color: black">1、在主线程成功注册 Service Worker 之后，开始下载并解析执行 Service Worker 文件，执行过程中开始安装 Service Worker，在此过程中会触发 worker 线程的 install 事件。</p>

<p style="text-shadow: none; color: black">2、如果 install 事件回调成功执行（在 install 回调中通常会做一些缓存读写的工作，可能会存在失败的情况），则开始激活 Service Worker，在此过程中会触发 worker 线程的 - activate 事件，如果 install 事件回调执行失败，则生命周期进入 Error 终结状态，终止生命周期。</p>

<p style="text-shadow: none; color: black">3、完成激活之后，Service Worker 就能够控制作用域下的页面的资源请求，可以监听 fetch 事件。</p>

<p style="text-shadow: none; color: black">4、如果在激活后 Service Worker 被 unregister 或者有新的 Service Worker 版本更新，则当前 Service Worker 生命周期完结，进入 Terminated 终结状态。</p>

<p style="text-shadow: none; color: black">通过 Service Worker 让 PWA...?</p>
:::


<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header{.alignright}
### Service Worker - 预缓存资源
:::

:::column
#### 在安装过程中缓存资源
```mermaid
graph TD
	register --> install -- open cache --> installed
```

---

<div style="height: 800px; overflow: auto;">

```js
// 缓存的key
const cacheName = 'joelblog-sorce-v0.0.2';

// 保存到本地cache的文件路径
const filesToCache = ['/', '/index.html', '/css/xx.css', '/js/xx.js', ...];

// 安装
self.addEventListener('install', function(e) {
	console.log('[ServiceWorker] Install');

	e.waitUntil(
		caches.open(cacheName).then(function(cache) {
			console.log('[ServiceWorker] Caching app shell');
			return cache.addAll(filesToCache);
		})
	);
});
```

</div>

:::

:::note
<p style="text-shadow: none; color: black">首先我们在 install 的监听函数中，利用Caches api 把定义好的资源列表缓存起来，完成安装以后，Service进入激活状态。</p>
:::

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### Service Worker - 拦截代理
:::

:::column
#### 在安装过程中缓存资源
```mermaid
graph LR
	UA --> fetch
	fetch --> UA
	fetch --> id1{match request}
	id1{match request} --> fetch
	id1{match request} -- Y --> Caches
	id1{match request} -- N --> Response
```

---

<div style="height: 800px; overflow: auto;">

```js
// 拦截请求
self.addEventListener('fetch', function(e) {
	console.log('[Service Worker] Fetch', e.request.url);

	e.respondWith(
		caches.match(e.request).then((response) => {
			return response || fetch(e.request);
		}),
	);
});
```

</div>

:::

:::note
<p style="text-shadow: none; color: black">当用户再次访问页面时，fetch 事件会拦截请求，只要匹配到缓存的key，优先返回缓存的内容，如果没有会拉取服务端数据返回</p>

<p style="text-shadow: none; color: black">目前demo存储管理，使用的是cache api</p>

<p style="text-shadow: none; color: black">Cache API 是为资源请求与响应的存储量身定做的，它采用了键值对的数据模型存储格式，以请求对象为键、响应对象为值，正好对应了发起网络资源请求时请求与响应一一对应的关系。因此 Cache API 适用于请求响应的本地存储。但除了Cache API，本地的存储管理还有 IndexedDB</p>

<p style="text-shadow: none; color: black">IndexedDB 是一种非关系型（NoSQL）数据库，它的存储对象主要是数据，比如数字、字符串、Plain Objects、Array 等，以及少量特殊对象比如 Date、RegExp、Map、Set 等等，对于 Request、Response 这些是无法直接被 IndexedDB 存储的。</p>

<p style="text-shadow: none; color: black">另外可以看到展示的代码里面，fetch 事件拦截后优先读取缓存返回给你客户端，否则请求服务器数据，在实际的应用中，要跟据业务情况来制定我们的缓存策略。一般有以下几种（切）</p>


<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### Service Worker - 缓存策略
:::

:::column {.build}

### NetworkFirst
网络优先

### CacheFirst
缓存优先


---

### NetworkOnly
仅使用正常的网络请求

### CacheOnly
仅使用缓存中的资源

---

### StaleWhileRevalidate
从缓存中读取资源的同时发送网络请求更新本地缓存

:::

:::note
<p style="text-shadow: none; color: black">首先是比较常用的网络优先，这种方式能以最新的资源展示页面，但在网络请求失败的情况下也能读取缓存，另一个则是相反，优先读取缓存内容，这里的优势就是速度够快，省去了请求等待时间，没有否发起资源请求。</p>
<p style="text-shadow: none; color: black">其次就是单一的读取方案，要么是请求数据，要么是缓存中的数据，2选1</p>
<p style="text-shadow: none; color: black">最后一个也比较有代表性的存储策略，从缓存中读取资源的同时发送网络请求更新本地缓存，即保证以最快的速度返回内容进行渲染，同时在线程中进行资源更新，优点是快，不足的在于如果资源进行了更新，当次访问只会进行缓存，等下次访问才会是最新的内容。</p>
<p style="text-shadow: none; color: black">到这里 service worker 介绍得差不多，我们来展示一下效果</p>

<p style="text-shadow: none; color: black">由于其在后台独立运行的特性，它不会阻塞浏览器脚本的运行，同时也无法直接访问浏览器相关的API（例如：DOM、localStorage等）。此外，即使在离开你的Web App，甚至是关闭浏览器后，它仍然可以运行。就像是一个在Web应用背后默默工作的勤劳小蜜蜂，所以除了可以处理缓存之外，还能推送、通知消息。</p>
:::

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::{.content-right}
## Web Push
- 推送消息（Push）{.animated.fadeInUp.delay-400}
- 展示提醒（Notification）{.animated.fadeInUp.delay-800}
:::

:::note
<p style="text-shadow: none; color: black">在 iOS 和 Android 移动设备中，Native App 向用户推送通知是很常见的行为，这是重新吸引用户访问应用最有效方法之一。然而推送通知一直被认为是 Web App 缺少的能力</p>
:::

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### Web Push
:::

:::div {.text-cols}
##### 角色
```{.animated.fadeInUp.delay-400}
    +-------+           +--------------+       +-------------+
    |  UA   |           | Push Service |       | Application |
    +-------+           +--------------+       |   Server    |
                                               +-------------+
```

---

##### 在Push中登场的三个重要“角色”分别是：
* 浏览器{.animated.fadeInRight}
* Push Service：专门的Push服务，你可以认为是一个第三方服务，目前chrome与firefox都有自己的Push Service Service。理论上只要浏览器支持，可以使用任意的Push Service{.animated.fadeInRight.delay-400}
* 后端服务：这里就是指我们自己的后端服务{.animated.fadeInRight.delay-600}

:::

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### Web Push
:::

:::div {.text-cols}
##### 消息推送流程
```{.animated.fadeInUp.delay-400}
    +-------+           +--------------+       +-------------+
    |  UA   |           | Push Service |       | Application |
    +-------+           +--------------+       |   Server    |
        |                      |               +-------------+
        |      Subscribe       |                      |
        |--------------------->|                      |
        |       Monitor        |                      |
        |<====================>|                      |
        |                      |                      |
        |          Distribute Push Resource           |
        |-------------------------------------------->|
        |                      |                      |
```
---

##### subscribe，首先是订阅：
* Ask Permission：这一步不再上图的流程中，这其实是浏览器中的策略。浏览器会询问用户是否允许通知，只有在用户允许后，才能进行后面的操作{.animated.fadeInRight}
* Subscribe：浏览器（客户端）需要向Push Service发起订阅（subscribe），订阅后会得到一个PushSubscription对象{.animated.fadeInRight.delay-400}
* Monitor：订阅操作会和Push Service进行通信，生成相应的订阅信息，Push Service会维护相应信息，并基于此保持与客户端的联系{.animated.fadeInRight.delay-500}
* Distribute Push Resource：浏览器订阅完成后，会获取订阅的相关信息（存在于PushSubscription对象中），我们需要将这些信息发送到自己的服务端，在服务端进行保存{.animated.fadeInRight.delay-600}

:::

:::note
<p style="text-shadow: none; color: black">该时序图是Web Push的的第一个步骤，我们可以将其称为订阅（subscribe）</p>
:::

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::header
### Web Push
:::

:::div {.text-cols}
##### 消息推送流程
```{.animated.fadeInUp.delay-400}
    +-------+           +--------------+       +-------------+
    |  UA   |           | Push Service |       | Application |
    +-------+           +--------------+       |   Server    |
        |                      |               +-------------+
        |      Subscribe       |                      |
        |--------------------->|                      |
        |       Monitor        |                      |
        |<====================>|                      |
        |                      |                      |
        |          Distribute Push Resource           |
        |-------------------------------------------->|
        |                      |                      |
        :                      :                      :
        |                      |     Push Message     |
        |    Push Message      |<---------------------|
        |<---------------------|                      |
        |                      |                      |
```

##### Push Message，然后是推送：

* Push Message阶段一：我们的服务端需要推送消息时，不直接和客户端交互，而是通过Web Push协议，将相关信息通知Push Service {.animated.fadeInRight}
* Push Message阶段二：Push Service收到消息，通过校验后，基于其维护的客户端信息，将消息推送给订阅了的客户端 {.animated.fadeInRight.delay-400}
* 最后，客户端收到消息，完成整个推送过程 {.animated.fadeInRight.delay-500}
:::

:::note
<p style="text-shadow: none; color: black">其次就是推送（push）</p>

<p style="text-shadow: none; color: black">在上面的Push流程中，出现了一个比较少接触到的角色：Push Service。</p>

<p style="text-shadow: none; color: black">Push Service可以接收网络请求，校验该请求并将其推送给合适的浏览器客户端。Push Service还有一个非常重要的功能：当用户离线时，可以帮我们保存消息队列，直到用户联网后再发送给他们。</p>

<p style="text-shadow: none; color: black">目前，不同的浏览器厂商使用了不同的Push Service。例如，chrome使用了google自家的FCM（前身为GCM），firefox也是使用自家的服务。那么我们是否需要写不同的代码来兼容不同的浏览器所使用的服务呢？答案是并不用。Push Service遵循Web Push Protocol，其规定了请求及其处理的各种细节，这就保证了，不同的Push Service也会具有标准的调用方式。</p>

<p style="text-shadow: none; color: black">------------</p>

<p style="text-shadow: none; color: black">这里再提一点：我们在上一节中说了Push的标准流程，其中第一步就是浏览器发起订阅，生成一个PushSubscription对。Push Service会为每个发起订阅的浏览器生成一个唯一的URL，这样，我们在服务端推送消息时，向这个URL进行推送后，Push Service就会知道要通知哪个浏览器。而这个URL信息也在PushSubscription对象里，叫做endpoint。</p>

<p style="text-shadow: none; color: black"> 如何保证Push的安全性</p>

<p style="text-shadow: none; color: black"> 在Web Push中，为了保证客户端只会收到其订阅的服务端推送的消息（其他的服务端即使在拿到endpoint也无法推送消息），需要对推送信息进行数字签名。该过程大致如下：</p>

<p style="text-shadow: none; color: black"> 在Web Push中会有一对公钥与私钥。客户端持有公钥，而服务端持有私钥。客户端在订阅时，会将公钥发送给Push Service，而Push Service会将该公钥与相应的endpoint维护起来。而当服务端要推送消息时，会使用私钥对发送的数据进行数字签名，并根据数字签名生成一个叫】Authorization请求头。Push Service收到请求后，根据endpoint取到公钥，对数字签名解密验证，如果信息相符则表明该请求是通过对应的私钥加密而成，也表明该请求来自浏览器所订阅的服务端。反之亦然。</p>

:::

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::{.content-right}
> “PWA不是某一项技术，或者某一个新的产物；而是一系列Web技术与标准的集合与应用。通过应用这些新的技术与标准，可以从安全、性能和体验三个方面，优化我们的Web App。所以，其实PWA本质上依然是一个Web App。” 
> ==总结==
:::

:::note
<p style="text-shadow: none; color: black">回过头我们总结一下，什么是PWA？我们需要理解的是，PWA不是某一项技术，或者某一个新的产物；而是一系列Web技术与标准的集合与应用。通过应用这些新的技术与标准，可以从安全、性能和体验三个方面，优化我们的Web App。所以，其实PWA本质上依然是一个Web App。</p>
:::

<slide class="bg-black-blue" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
:::{.content-right}
> “That's all for today, thanks for listening!”
> ==by Congyu Shen==
:::

:::note
<p style="text-shadow: none; color: black">------------</p>
:::

# MoonTV (源碼 最後的版本)
## 很辛苦找到的 =p 歡迎 Fork ... 並更改其他名稱為專案名稱，以免被刪除。

<div align="center">
  <img src="public/logo.png" alt="LibreTV Logo" width="120">
</div>

> 🎬 **MoonTV** 是一個開箱即用的、跨平台的影視聚合播放器。它基于 **Next.js 14** + **Tailwind&nbsp;CSS** + **TypeScript** 構建，支持多資源搜索、在線播放、收藏同步、播放記錄、本地/雲端存儲，讓你可以隨時隨地暢享海量免費影視內容。

<div align="center">

![Next.js](https://img.shields.io/badge/Next.js-14-000?logo=nextdotjs)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-3-38bdf8?logo=tailwindcss)
![TypeScript](https://img.shields.io/badge/TypeScript-4.x-3178c6?logo=typescript)
![License](https://img.shields.io/badge/License-MIT-green)
![Docker Ready](https://img.shields.io/badge/Docker-ready-blue?logo=docker)

</div>

---

## ✨ 功能特性

- 🔍 **多源聚合搜索**：內置數十個免費資源站點，一次搜索立刻返回全源結果。
- 📄 **豐富詳情頁**：支持劇集列表、演員、年份、簡介等完整信息展示。
- ▶️ **流暢在線播放**：集成 HLS.js & ArtPlayer。
- ❤️ **收藏 + 繼續觀看**：支持 Redis/D1/Upstash 存儲，多端同步進度。
- 📱 **PWA**：離線緩存、安裝到桌面/主屏，移動端原生體驗。
- 🌗 **響應式布局**：桌面側邊欄 + 移動底部導航，自適應各種屏幕尺寸。
- 🚀 **極簡部署**：一條 Docker 命令即可將完整服務跑起來，或免費部署到 Vercel、Netlify 和 ~~Cloudflare~~。
- 👿 **智能去廣告**：自動跳過視頻中的切片廣告（實驗性）

<details>
  <summary>點擊查看項目截圖</summary>
  <img src="public/screenshot1.png" alt="項目截圖" style="max-width:600px">
  <img src="public/screenshot2.png" alt="項目截圖" style="max-width:600px">
  <img src="public/screenshot3.png" alt="項目截圖" style="max-width:600px">
</details>

## 🗺 目錄

- [技術棧](#技術棧)
- [部署](#部署)
- [Docker Compose 最佳實踐](#Docker-Compose-最佳實踐)
- [環境變量](#環境變量)
- [配置說明](#配置說明)
- [管理員配置](#管理員配置)
- [AndroidTV 使用](#AndroidTV-使用)
- [Roadmap](#roadmap)
- [安全與隱私提醒](#安全與隱私提醒)
- [License](#license)
- [致謝](#致謝)

## 技術棧

| 分類      | 主要依賴                                                                                              |
| --------- | ----------------------------------------------------------------------------------------------------- |
| 前端框架  | [Next.js 14](https://nextjs.org/) · App Router                                                        |
| UI & 樣式 | [Tailwind&nbsp;CSS 3](https://tailwindcss.com/)                                                       |
| 語言      | TypeScript 4                                                                                          |
| 播放器    | [ArtPlayer](https://github.com/zhw2590582/ArtPlayer) · [HLS.js](https://github.com/video-dev/hls.js/) |
| 代碼質量  | ESLint · Prettier · Jest                                                                              |
| 部署      | Docker · Vercel · CloudFlare pages                                                                    |

## 部署

本項目**支持 Vercel、Docker、Netlify 和 ~~Cloudflare~~** 部署。

存儲支持矩陣

|                   | Docker | Vercel | Netlify | ~~Cloudflare~~ |
| :---------------: | :----: | :----: | :-----: | :------------: |
|   localstorage    |   ✅   |   ✅   |   ✅    |       ✅       |
|    原生 redis     |   ✅   |        |         |                |
| ~~Cloudflare D1~~ |        |        |         |       ✅       |
|   Upstash Redis   |   ☑️   |   ✅   |   ✅    |       ☑️       |

✅：經測試支持

☑️：理論上支持，未測試

除 localstorage 方式外，其他方式都支持多賬戶、記錄同步和管理頁面

### Vercel 部署

#### 普通部署（localstorage）

1. **Fork** 本倉庫到你的 GitHub 賬戶。
2. 登陸 [Vercel](https://vercel.com/)，點擊 **Add New → Project**，選擇 Fork 後的倉庫。
3. 設置 PASSWORD 環境變量。
4. 保持默認設置完成首次部署。
5. 如需自定義 `config.json`，請直接修改 Fork 後倉庫中該文件。
6. 每次 Push 到 `main` 分支將自動觸發重新構建。

部署完成後即可通過分配的域名訪問，也可以綁定自定義域名。

#### Upstash Redis 支持

0. 完成普通部署並成功訪問。
1. 在 [upstash](https://upstash.com/) 注冊賬號並新建一個 Redis 實例，名稱任意。
2. 複制新數據庫的 **HTTPS ENDPOINT 和 TOKEN**
3. 返回你的 Vercel 項目，新增環境變量 **UPSTASH_URL 和 UPSTASH_TOKEN**，值爲第二步複制的 endpoint 和 token
4. 設置環境變量 NEXT_PUBLIC_STORAGE_TYPE，值爲 **upstash**；設置 USERNAME 和 PASSWORD 作爲站長賬號
5. 重試部署

### Netlify 部署

#### 普通部署（localstorage）

1. **Fork** 本倉庫到你的 GitHub 賬戶。
2. 登陸 [Netlify](https://www.netlify.com/)，點擊 **Add New project → Importing an existing project**，授權 Github，選擇 Fork 後的倉庫。
3. 設置 PASSWORD 環境變量。
4. 保持默認設置完成首次部署。
5. 如需自定義 `config.json`，請直接修改 Fork 後倉庫中該文件。
6. 每次 Push 到 `main` 分支將自動觸發重新構建。

部署完成後即可通過分配的域名訪問，也可以綁定自定義域名。

#### Upstash Redis 支持

0. 完成普通部署並成功訪問。
1. 在 [upstash](https://upstash.com/) 注冊賬號並新建一個 Redis 實例，名稱任意。
2. 複制新數據庫的 **HTTPS ENDPOINT 和 TOKEN**
3. 返回你的 Netlify 項目，**Project Configuration → Environment variables** 新增環境變量 **UPSTASH_URL 和 UPSTASH_TOKEN**，值爲第二步複制的 endpoint 和 token
4. 設置環境變量 NEXT_PUBLIC_STORAGE_TYPE，值爲 **upstash**；設置 USERNAME 和 PASSWORD 作爲站長賬號
5. 重試部署

### Cloudflare 部署（**不支持，詳情請看置頂 issue**）

~~**Cloudflare Pages 的環境變量盡量設置爲密鑰而非文本**~~

#### ~~普通部署（localstorage）~~

~~1. **Fork** 本倉庫到你的 GitHub 賬戶。~~
~~2. 登陸 [Cloudflare](https://cloudflare.com)，點擊 **計算（Workers）-> Workers 和 Pages**，點擊創建~~
~~3. 選擇 Pages，導入現有的 Git 存儲庫，選擇 Fork 後的倉庫~~
~~4. 構建命令填寫 **pnpm install --frozen-lockfile && pnpm run pages:build**，預設框架爲無，**構建輸出目錄**爲 `.vercel/output/static`~~
~~5. 保持默認設置完成首次部署。進入設置，將兼容性標志設置爲 `nodejs_compat`，無需選擇，直接粘貼~~
~~6. 首次部署完成後進入設置，新增 PASSWORD 密鑰（變量和機密下），而後重試部署。~~
~~7. 如需自定義 `config.json`，請直接修改 Fork 後倉庫中該文件。~~
~~8. 每次 Push 到 `main` 分支將自動觸發重新構建。~~

#### ~~D1 支持~~

~~0. 完成普通部署並成功訪問~~
~~1. 點擊 **存儲和數據庫 -> D1 SQL 數據庫**，創建一個新的數據庫，名稱隨意~~
~~2. 進入剛創建的數據庫，點擊左上角的 Explore Data，將[D1 初始化](D1初始化.md) 中的內容粘貼到 Query 窗口後點擊 **Run All**，等待運行完成~~
~~3. 返回你的 pages 項目，進入 **設置 -> 綁定**，添加綁定 D1 數據庫，選擇你剛創建的數據庫，變量名稱填 **DB**~~
~~4. 設置環境變量 NEXT_PUBLIC_STORAGE_TYPE，值爲 **d1**；設置 USERNAME 和 PASSWORD 作爲站長賬號~~
~~5. 重試部署~~

### Docker 部署

#### 1. 直接運行（最簡單，localstorage）

```bash
# 拉取預構建鏡像
# 推薦使用具體版本號標簽，確保穩定性
docker pull ghcr.io/lunatechlab/moontv:1.1.1
# 或拉取最新版本
docker pull ghcr.io/tharinwong/moontv:latest

# 運行容器
# -d: 後台運行  -p: 映射端口 3000 -> 3000
docker run -d --name moontv -p 3000:3000 --env PASSWORD=your_password ghcr.io/lunatechlab/moontv:latest
```

#### 可用標簽

- `ghcr.io/tharinwong/moontv:1.1.1` - 具體版本號，推薦用于生産環境
- `ghcr.io/tharinwong/moontv:latest` - 最新版本，可能包含最新功能但也可能有未測試的變化
- `ghcr.io/tharinwong/moontv:pr-{number}` - PR 構建版本，用于測試新功能

訪問 `http://服務器 IP:3000` 即可。（需自行到服務器控制台放通 `3000` 端口）

## Docker Compose 最佳實踐

若你使用 docker compose 部署，以下是一些 compose 示例

### local storage 版本

```yaml
services:
  moontv-core:
    image: ghcr.io/tharinwong/moontv:latest
    container_name: moontv-core
    restart: unless-stopped
    ports:
      - '3000:3000'
    environment:
      - PASSWORD=your_password
    # 如需自定義配置，可挂載文件
    # volumes:
    #   - ./config.json:/app/config.json:ro
```

### Redis 版本（推薦，多賬戶數據隔離，跨設備同步）

```yaml
services:
  moontv-core:
    image: ghcr.io/tharinwong/moontv:latest
    container_name: moontv-core
    restart: unless-stopped
    ports:
      - '3000:3000'
    environment:
      - USERNAME=admin
      - PASSWORD=admin_password
      - NEXT_PUBLIC_STORAGE_TYPE=redis
      - REDIS_URL=redis://moontv-redis:6379
      - NEXT_PUBLIC_ENABLE_REGISTER=true
    networks:
      - moontv-network
    depends_on:
      - moontv-redis
    # 如需自定義配置，可挂載文件
    # volumes:
    #   - ./config.json:/app/config.json:ro
  moontv-redis:
    image: redis:alpine
    container_name: moontv-redis
    restart: unless-stopped
    networks:
      - moontv-network
    # 如需持久化
    # volumes:
    #   - ./data:/data
networks:
  moontv-network:
    driver: bridge
```

## 自動同步最近更改

建議在 fork 的倉庫中啓用本倉庫自帶的 GitHub Actions 自動同步功能（見 `.github/workflows/sync.yml`）。

如需手動同步主倉庫更新，也可以使用 GitHub 官方的 [Sync fork](https://docs.github.com/cn/github/collaborating-with-issues-and-pull-requests/syncing-a-fork) 功能。

## 環境變量

| 變量                                | 說明                                         | 可選值                           | 默認值                                                                                                                     |
| ----------------------------------- | -------------------------------------------- | -------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| USERNAME                            | 非 localstorage 部署時的管理員賬號           | 任意字符串                       | （空）                                                                                                                     |
| PASSWORD                            | 非 localstorage 部署時爲管理員密碼           | 任意字符串                       | （空）                                                                                                                     |
| NEXT_PUBLIC_SITE_NAME               | 站點名稱                                     | 任意字符串                       | MoonTV                                                                                                                     |
| ANNOUNCEMENT                        | 站點公告                                     | 任意字符串                       | 本網站僅提供影視信息搜索服務，所有內容均來自第三方網站。本站不存儲任何視頻資源，不對任何內容的准確性、合法性、完整性負責。 |
| NEXT_PUBLIC_STORAGE_TYPE            | 播放記錄/收藏的存儲方式                      | localstorage、redis、d1、upstash | localstorage                                                                                                               |
| REDIS_URL                           | redis 連接 url                               | 連接 url                         | 空                                                                                                                         |
| UPSTASH_URL                         | upstash redis 連接 url                       | 連接 url                         | 空                                                                                                                         |
| UPSTASH_TOKEN                       | upstash redis 連接 token                     | 連接 token                       | 空                                                                                                                         |
| NEXT_PUBLIC_ENABLE_REGISTER         | 是否開放注冊，僅在非 localstorage 部署時生效 | true / false                     | false                                                                                                                      |
| NEXT_PUBLIC_SEARCH_MAX_PAGE         | 搜索接口可拉取的最大頁數                     | 1-50                             | 5                                                                                                                          |
| NEXT_PUBLIC_DOUBAN_PROXY_TYPE       | 豆瓣數據源請求方式                           | 見下方                           | direct                                                                                                                     |
| NEXT_PUBLIC_DOUBAN_PROXY            | 自定義豆瓣數據代理 URL                       | url prefix                       | (空)                                                                                                                       |
| NEXT_PUBLIC_DOUBAN_IMAGE_PROXY_TYPE | 豆瓣圖片代理類型                             | 見下方                           | direct                                                                                                                     |
| NEXT_PUBLIC_DOUBAN_IMAGE_PROXY      | 自定義豆瓣圖片代理 URL                       | url prefix                       | (空)                                                                                                                       |
| direct                              |
| NEXT_PUBLIC_DISABLE_YELLOW_FILTER   | 關閉色情內容過濾                             | true/false                       | false                                                                                                                      |

NEXT_PUBLIC_DOUBAN_PROXY_TYPE 選項解釋：

- direct: 由服務器直接請求豆瓣源站
- cors-proxy-zwei: 浏覽器向 cors proxy 請求豆瓣數據，該 cors proxy 由 [Zwei](https://github.com/bestzwei) 搭建
- cmliussss-cdn-tencent: 浏覽器向豆瓣 CDN 請求數據，該 CDN 由 [CMLiussss](https://github.com/cmliu) 搭建，並由騰訊雲 cdn 提供加速
- cmliussss-cdn-ali: 浏覽器向豆瓣 CDN 請求數據，該 CDN 由 [CMLiussss](https://github.com/cmliu) 搭建，並由阿裏雲 cdn 提供加速
- cors-anywhere: 浏覽器向 cors proxy 請求豆瓣數據，該 cors proxy 爲公共服務 [cors-anywhere](https://cors-anywhere.com)，限制每分鍾 20 次請求
- custom: 用戶自定義 proxy，由 NEXT_PUBLIC_DOUBAN_PROXY 定義

NEXT_PUBLIC_DOUBAN_IMAGE_PROXY_TYPE 選項解釋：

- direct：由浏覽器直接請求豆瓣分配的默認圖片域名
- server：由服務器代理請求豆瓣分配的默認圖片域名
- img3：由浏覽器請求豆瓣官方的精品 cdn（阿裏雲）
- cmliussss-cdn-tencent：由浏覽器請求豆瓣 CDN，該 CDN 由 [CMLiussss](https://github.com/cmliu) 搭建，並由騰訊雲 cdn 提供加速
- cmliussss-cdn-ali：由浏覽器請求豆瓣 CDN，該 CDN 由 [CMLiussss](https://github.com/cmliu) 搭建，並由阿裏雲 cdn 提供加速
- custom: 用戶自定義 proxy，由 NEXT_PUBLIC_DOUBAN_IMAGE_PROXY 定義

## 配置說明

所有可自定義項集中在根目錄的 `config.json` 中：

```json
{
  "cache_time": 7200,
  "api_site": {
    "dyttzy": {
      "api": "http://www.google.com/api.php/provide/vod",
      "name": "Google API",
      "detail": "http://api.google.com"
    }
    // ...更多站點
  },
  "custom_category": [
    {
      "name": "華語",
      "type": "movie",
      "query": "華語"
    }
  ]
}
```

- `cache_time`：接口緩存時間（秒）。
- `api_site`：你可以增刪或替換任何資源站，字段說明：
  - `key`：唯一標識，保持小寫字母/數字。
  - `api`：資源站提供的 `vod` JSON API 根地址。
  - `name`：在人機界面中展示的名稱。
  - `detail`：（可選）部分無法通過 API 獲取劇集詳情的站點，需要提供網頁詳情根 URL，用于爬取。
- `custom_category`：自定義分類配置，用于在導航中添加個性化的影視分類。以 type + query 作爲唯一標識。支持以下字段：
  - `name`：分類顯示名稱（可選，如不提供則使用 query 作爲顯示名）
  - `type`：分類類型，支持 `movie`（電影）或 `tv`（電視劇）
  - `query`：搜索關鍵詞，用于在豆瓣 API 中搜索相關內容

custom_category 支持的自定義分類已知如下：

- movie：熱門、最新、經典、豆瓣高分、冷門佳片、華語、歐美、韓國、日本、動作、喜劇、愛情、科幻、懸疑、恐怖、治愈
- tv：熱門、美劇、英劇、韓劇、日劇、國産劇、港劇、日本動畫、綜藝、紀錄片

也可輸入如 "哈利波特" 效果等同于豆瓣搜索

MoonTV 支持標准的蘋果 CMS V10 API 格式。

修改後 **無需重新構建**，服務會在啓動時讀取一次。

## 管理員配置

**該特性目前僅支持通過非 localstorage 存儲的部署方式使用**

支持在運行時動態變更服務配置

設置環境變量 USERNAME 和 PASSWORD 即爲站長用戶，站長可設置用戶爲管理員

站長或管理員訪問 `/admin` 即可進行管理員配置

## AndroidTV 使用

目前該項目可以配合 [OrionTV](https://github.com/zimplexing/OrionTV) 在 Android TV 上使用，可以直接作爲 OrionTV 後端

暫時收藏夾與播放記錄和網頁端隔離，後續會支持同步用戶數據

## Roadmap

- [x] 深色模式
- [x] 持久化存儲
- [x] 多賬戶

## 安全與隱私提醒

### 請設置密碼保護並關閉公網注冊

爲了您的安全和避免潛在的法律風險，我們要求在部署時設置密碼保護並**強烈建議關閉公網注冊**：

- **避免公開訪問**：不設置密碼的實例任何人都可以訪問，可能被惡意利用
- **防範版權風險**：公開的視頻搜索服務可能面臨版權方的投訴舉報
- **保護個人隱私**：設置密碼可以限制訪問範圍，保護您的使用記錄

### 部署要求

1. **設置環境變量 `PASSWORD`**：爲您的實例設置一個強密碼
2. **僅供個人使用**：請勿將您的實例鏈接公開分享或傳播
3. **遵守當地法律**：請確保您的使用行爲符合當地法律法規

### 重要聲明

- 本項目僅供學習和個人使用
- 請勿將部署的實例用于商業用途或公開服務
- 如因公開分享導致的任何法律問題，用戶需自行承擔責任
- 項目開發者不對用戶的使用行爲承擔任何法律責任

## License

[MIT](LICENSE) © 2025 MoonTV & Contributors

## 致謝

- [ts-nextjs-tailwind-starter](https://github.com/theodorusclarence/ts-nextjs-tailwind-starter) — 項目最初基于該腳手架。
- [LibreTV](https://github.com/LibreSpark/LibreTV) — 由此啓發，站在巨人的肩膀上。
- [ArtPlayer](https://github.com/zhw2590582/ArtPlayer) — 提供強大的網頁視頻播放器。
- [HLS.js](https://github.com/video-dev/hls.js) — 實現 HLS 流媒體在浏覽器中的播放支持。
- [Zwei](https://github.com/bestzwei) — 提供獲取豆瓣數據的 cors proxy
- [CMLiussss](https://github.com/cmliu) — 提供豆瓣 CDN 服務
- 感謝所有提供免費影視接口的站點。

- [Github - senshinya](https://github.com/senshinya) - 該專案主人

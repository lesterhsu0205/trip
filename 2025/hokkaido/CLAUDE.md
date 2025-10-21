# CLAUDE.md - 北海道行程專案

本檔案為 Claude Code 在處理北海道行程時的專項指引。

## 專案概述

這是一個家庭旅遊規劃儲存庫，針對 2025 年 11/14-11/19 的北海道之旅。儲存庫包含詳細的行程文件，專注於帶 1 歲男幼兒和 6 歲男童的家庭旅遊。

## 專案檔案結構

```text
.
├── README.md             # 主要行程檔案 (自動合併版本)
├── CLAUDE.md             # Claude Code 子專案指引
├── todo.md               # 行程前購票及預定等整理
├── remark.md             # 行程中購物資訊及地點整理
├── transportation.md     # 行程中大眾交通資訊整理
├── D1.md                 # 各日詳細行程
├── D2.md
├── ...
└── D8.md
```

## 專案特殊規則

- **當地語言標註**： 有片假名就不需提供平假名，除非只有平假名

## 專案詳細資訊

### 1. **家庭成員配置**

- **成員**： 30+ 歲夫妻 + 1 歲男幼兒 + 6 歲男童 + 37 歲男性 + 60 歲女性
- **特殊需求**：
  - 1 歲幼兒：需嬰兒車、不宜太累的行程
  - 6 歲男童：需適合身高 110 公分的設施

### 2. **交通資訊**

#### 重點資訊

- **航班資訊**：
  - 去程：台灣虎航 IT234｜11/14 06:20（TPE terminal 1）→ 10:55（CTS）
  - 回程：台灣虎航 IT235｜11/19 11:55（CTS）→ 15:35（TPE terminal 1）
- **當地交通原則**：
  - 北海道：租車自駕
- **時間安排要求**：
  - 最少 buffer：10 分鐘
  - 違反者提醒是否重新安排時程

#### **交通資訊查詢要點**

##### **[起點] 與 [終點] 地址格式要求**

- **查詢交通的 [起點] 與 [終點] 皆用日文帶入**
- **[NAVITIME] 僅接受日文地址或日文地標名稱**
- 住宿地址：使用 [住宿資訊](#3-住宿資訊) 中標註的日文地址（如「長野県北佐久郡軽井沢町軽井沢（大字）1314-29」）
- 景點地址：使用日文地標名稱或完整日文地址
- ✅ 正確：使用完整日文地址或日文地標名稱
- ❌ 錯誤：使用英文地址或混合格式

##### **交通資訊查詢結果格式**

- **NAVITIME（自駕）**
  - 必須標註：「NAVITIME [路線類型]，[行駛時間] 分鐘，[距離]km，ETC 過路費 ¥[費用]，預估油耗 [油耗]L」
  - 路線類型：「最佳路線」與「免費道路」
  - 必須顯示兩種路線選項
- **NAVITIME URL**：[參考](#navitime-url)
- **Google Maps URL**：[參考](#google-maps-url)
- 依照[查證標註要求](./../../CLAUDE.md#查證標註要求)格式標註

##### Google Maps URL

- 建構方式
  1. **判斷 [起點] 與 [終點]**：[參考](#起點-與-終點-地址格式要求)
  2. **判斷 [模式]**（根據行程情境自動選擇）
      - `transit`：城市間移動、跨區域移動、明確提到電車/地鐵/巴士
      - `walking`：距離 < 1km、同一區域內、景點間步行
      - `driving`：郊區景點、租車行程、明確提到開車
  3. **帶入參數並建構 Google Maps URL**

- 格式：🗺️ [Google Maps 導航](https://www.google.com/maps/dir/?api=1&origin=[起點]&destination=[終點]&travelmode=[模式])
- 範例：🗺️ [Google Maps 導航](https://www.google.com/maps/dir/?api=1&origin=四ツ木駅&destination=台場駅&travelmode=transit)

##### NAVITIME URL

- 建構方式
  1. **判斷 [起點] 與 [終點]**：[參考](#起點-與-終點-地址格式要求)
  2. **查詢 [起點]、[終點] 經緯度**：[使用 WebSearch 查詢地點的 lat/lon 座標](#經緯度查詢方法)
  3. **建構 JSON 格式座標**
     - 起點：{"name":"[起點]","lat":起點緯度,"lon":起點經度}
     - 終點：{"name":"[終點]","lat":終點緯度,"lon":終點經度}
  4. **判斷 [出發時間]**：YYYY-MM-DDTHH%3Amm%3Ass
  5. **自駕查詢必須同時取得兩種路線資訊**：
     - 最佳路線（含收費道路）
     - 免費道路路線（避開收費道路）
  6. **帶入參數並建構 NAVITIME URL**

- 格式：🚗 [NAVITIME 自駕導航](https://www.navitime.co.jp/maps/routeResult?start={"name":"[起點]","lat":緯度,"lon":經度}&goal={"name":"[終點]","lat":緯度,"lon":經度}&car=only.multi.turn&type=car&rough-estimate=taxi.co2&start-time=[出發時間])
- 範例：🚗 [NAVITIME 自駕導航](https://www.navitime.co.jp/maps/routeResult?start={"name":"軽井沢駅","lat":36.342636,"lon":138.635179}&goal={"name":"雲場池","lat":36.352091,"lon":138.62691}&car=only.multi.turn&type=car&rough-estimate=taxi.co2&start-time=2025-10-10T11%3A35%3A00)

###### **經緯度查詢方法**

當需要建構 NAVITIME URL 時，使用以下步驟查詢座標：

1. **WebSearch 查詢格式**：`"[地點名]" 緯度 経度 座標`
2. **提取座標資訊**：從搜尋結果中找到 lat/lon 數值
3. **建構 JSON**：按照格式 `{"name":"地點名","lat":緯度數值,"lon":經度數值}` 組成

**注意**：由於 NAVITIME 需要精確座標，建議同時使用 WebSearch 查證路線的基本資訊（時間、距離、停車場等）作為補充。

##### **自駕資訊查詢標準 Prompt**

```使用以下標準 prompt 向 NAVITIME WebFetch 查詢自駕路線資訊

請提供從 [起點] 到 [終點] 的自駕路線資訊，包括：

**最佳路線（含收費道路）**：
1. 行駛時間（分鐘）
2. 行駛距離（公里）
3. ETC 過路費（日圓）
4. 預估油耗（公升）
5. 主要經過道路名稱

**免費道路路線（避開收費道路）**：
1. 行駛時間（分鐘）
2. 行駛距離（公里）
3. 預估油耗（公升）
4. 主要經過道路名稱

**家庭出行建議**：
- 推薦路線選擇（最佳路線 vs 免費道路）
- 休息站建議（如有長距離駕駛）
- 停車場資訊（如適用）

請用繁體中文回答，並按照 NAVITIME 網站的實際資訊提供。
```

#### **官方參考交通資料**

1. [Google Maps](https://maps.google.com) - 輔助確認
   - 查詢方式：WebFetch 查詢 [Google Maps URL](#google-maps-url)
2. 自駕路線資訊：
   - [NAVITIME](https://www.navitime.co.jp/)

### 3. **住宿資訊**

1. 11/14-11/15｜[雅亭溫泉飯店](http://www.miyabitei.jp)
   - 地址：〒059-0551 北海道登別市登別温泉町100
2. 11/15-11/17｜
   - 地址：
3. 11/17-11/19｜[Minn 千歳](https://staytuned.asia/ja/brands/minn/hotels/minn-chitose)
   - 地址：北海道千歳市柏台南2-1-1

### 4. **景點/旅遊資訊**

- [Northern Horse Park 北國優駿公園](https://www.northern-horsepark.jp)
  - [票價](https://www.northern-horsepark.jp/pdf/price/pricelist.pdf)

# 專案規劃書：能量秘境 (Energy Secret Spots)

## 1. 專案願景與定位
* **品牌名稱**：能量秘境 (Energy Secret Spots)
* **Slogan**：探索台灣大自然的天然充電站
* **核心價值**：結合 Outdoor 探索與身心靈療癒，將「地理數據」轉譯為「能量指引」。
* **市場定位**：
    * **大眾層**：尋找週末出遊、避開人潮的「秘境」遊客。
    * **需求層**：感到焦慮、需要放鬆、或單純想「轉運/求好磁場」的現代人。
    * **系列關聯**：與原本的「衝浪（動態）」、「露營（靜態）」形成互補，此站專注於「內在/頻率」。

---

## 2. 資料架構與煉金術 (Data Architecture)
本專案不依賴即時天氣 API（如衝浪站的氣象），而是依賴 **靜態地理數據 + 演算法標記**。

### 2.1 資料來源 (Source)
* **主要來源**：OpenStreetMap (OSM)
* **工具庫**：Python `osmnx`
* **抓取目標 (Raw Data)**：
    * `natural=waterfall` (瀑布)
    * `natural=peak` (山峰/小百岳)
    * `landuse=forest` (森林步道)
    * `amenity=place_of_worship` + `location=mountain` (山中廟宇)
    * `natural=hot_spring` (野溪溫泉)

### 2.2 能量演算法 (The Alchemy Layer)
需在 Python 腳本中實作下列邏輯，將 OSM Tags 轉換為 `energy_profile`：

| 五行屬性 | 判定邏輯 (OSM Tags) | 能量關鍵字 (Keywords) | 推薦活動 |
| :--- | :--- | :--- | :--- |
| **水 (Water)** | `waterfall`, `river`, `spring`, `coast` | 淨化、流動、財運、釋放 | 冥想、聽水聲、喊叫 |
| **木 (Wood)** | `forest`, `park`, `tree`, `botanical_garden` | 生機、療癒、舒緩、創意 | 森林浴、擁抱大樹、瑜伽 |
| **火 (Fire)** | `hot_spring`, `sunny`, `volcano` | 動力、熱情、桃花、驅邪 | 泡湯、日曬、快走 |
| **土 (Earth)** | `peak`, `rock`, `stone`, `mountain_range` | 穩定、歸屬、自信、紮根 | 登山、赤腳踩地(Earthing)、堆石 |
| **金 (Metal)** | `place_of_worship`, `historic`, `cave` | 靈感、肅穆、決斷、集中 | 參拜、靜坐、寫作 |

### 2.3 資料庫 Schema (JSON Structure)
對應 `src/data/surf_spots.json`，新結構 `energy_spots.json` 如下：

```json
{
  "id": "osm_node_12345",
  "name": "五峰旗瀑布",
  "location": {
    "lat": 24.83,
    "lng": 121.75,
    "city": "宜蘭縣",
    "district": "礁溪鄉"
  },
  "tags": ["waterfall", "nature"],
  "energy_profile": {
    "primary": "Water",       // 主屬性
    "secondary": "Wood",      // 副屬性
    "radar_chart": [9, 5, 2, 4, 8], // [淨化, 療癒, 靈感, 穩定, 財運] (1-10分)
    "seclusion_level": 4,     // 秘境指數 (1: 熱鬧, 5: 無人)
    "keywords": ["強力淨化", "負離子爆棚", "招財水流"]
  },
  "best_time": "上午 09:00 - 11:00 (陽氣最盛)",
  "weather_tips": "雨後水量豐沛，能量最強",
  "affiliate": {
    "klook_link": "...",
    "accommodation_link": "..."
  }
}
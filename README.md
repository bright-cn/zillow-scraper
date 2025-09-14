# Zillow 数据抓取器

[![宣传图](https://github.com/bright-cn/LinkedIn-Scraper/blob/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.com/products/web-scraper/zillow) 

本仓库提供两种抓取 Zillow 数据的方法：
1. 免费的小规模抓取器，用于基础数据收集
2. 面向企业的大规模数据提取 API 方案

## 目录
- [免费 Zillow 数据抓取器](#free-zillow-data-scraper)
- [免费抓取器的限制](#limitations-of-free-scraper)
- [Zillow 抓取 API](#zillow-scraper-api)
  - [核心功能](#key-features)
  - [快速开始](#quick-start-guide)
  - [通过 URL 获取 Zillow 房源详情](#1-zillow-property-details-by-url)
  - [通过筛选条件获取 Zillow 房源列表](#2-zillow-properties-listing-by-filters)
  - [通过 URL 获取 Zillow 房源列表](#3-zillow-properties-listing-by-url)
  - [Zillow 价格历史](#4-zillow-price-history)
- [无代码抓取选项](#no-code-scraper-option)
- [其他可选项](#additional-options)
- [支持与资源](#support--resources)

## 免费 Zillow 数据抓取器
免费抓取器可用于在小规模范围内从 Zillow 的搜索页面收集房源数据。

### 输入要求
| 参数 | 必填 | 描述 |
|------|------|------|
| coords | 是 | 边界坐标 [西, 东, 南, 北] |
| pages  | 是 | 要抓取的页数 |

### 实现
使用抓取器时，请根据你的地理位置和数据需求，在以下代码中修改坐标和页数：
```python
# free_zillow_scraper/property_data.py
def get_search_params():
    return (
        -118.668176,  # 西经
        -118.155289,  # 东经
        33.703652,    # 南纬
        34.337306,    # 北纬
        5             # 要抓取的页数
    )
```

提示：任意位置的 Zillow 搜索页都可在 <script> 标签中找到地理坐标。查找以下标签：
```bash
<script id="__NEXT_DATA__" type="application/json">
```

### 示例输出
```json
{
    "id": "20595672",
    "price": "$1,599,900",
    "zestimate": 1605500,
    "location": {
        "address": "2215 Wellington Rd, Los Angeles, CA 90016",
        "city": "Los Angeles",
        "state": "CA",
        "zip": "90016",
        "coordinates": {"lat": 34.036064, "lon": -118.33622},
    },
    "details": {
        "beds": 4,
        "baths": 3.0,
        "area_sqft": 1886,
        "lot_acres": 8577.0,
        "property_type": "SINGLE_FAMILY",
    },
    "listing": {
        "status": "House for sale",
        "days_on_zillow": 5,
        "broker": "ehomes",
        "url": "https://www.zillow.com/homedetails/2215-Wellington-Rd-Los-Angeles-CA-90016/20595672_zpid/",
    },
},
```

## 免费抓取器的限制
免费 Zillow 抓取器适合小规模数据提取，但存在以下限制：

- 速率限制：抓取几次后 Zillow 会限制请求。
- IP 封禁：同一 IP 频繁抓取可能被封禁。
- 可扩展性有限：不适合高容量数据收集。
- 验证码：Zillow 可能通过验证码阻止自动化请求。
- 蜜罐：Zillow 使用蜜罐机制识别并拦截机器人。

若需大规模抓取，请考虑使用下方的 Zillow 抓取 API。

## Zillow 抓取 API
Bright Data 的 [Zillow 抓取 API](https://www.bright.cn/products/web-scraper/zillow) 可在无需自建和维护基础设施的情况下，提供可扩展、可靠、免操心的大规模 Zillow 数据提取方案。

### 核心功能
- 可扩展且可靠：针对高吞吐与实时采集优化。
- 反封锁：内置代理轮换与验证码处理。
- 合规：完全符合 GDPR 与 CCPA。
- 全球覆盖：可访问任意地区与语言的数据。
- 实时数据：低延迟的新鲜数据。
- 高级筛选：通过精确过滤器自定义采集。
- 按用量计费：仅为成功响应付费。
- 免费试用：赠送 20 次免费 API 调用。
- 7x24 支持：提供全天候技术支持。
- 无代码选项：支持通过 API 或无代码抓取器采集 Zillow 数据。

### 快速开始
- 注册：创建一个 [Bright Data 账户](https://www.bright.cn/)。
- 获取 API 令牌：在控制台获取你的 [API key](https://docs.brightdata.com/general/account/api-token)。
- 选择接口：从下方可用的 API 端点中进行选择。

## 1. 通过 URL 获取 Zillow 房源详情
通过提供房源 URL 来收集房源详情。

<img width="700" alt="zillow-房源列表信息" src="https://github.com/bright-cn/zillow-scraper/blob/main/zillow-images/zillow-properties-listing-information.png" />

### 输入参数
| 参数 | 必填 | 描述 |
|------|------|------|
| `url` | 是 | Zillow 房源 URL |

### 示例请求
#### Python 代码：
```python
properties = [
    {"url": "https://www.zillow.com/homedetails/73-Beverly-Park-Ln-Beverly-Hills-CA-90210/20533547_zpid/"},
    {"url": "https://www.zillow.com/homedetails/1945-N-Edgemont-St-Los-Angeles-CA-90027/20809871_zpid/"}
]
```

👉 完整 Python 脚本：[zillow_properties.py](https://github.com/bright-cn/Zillow-Scraper/blob/main/zillow_api_scraper/zillow_properties.py)

#### cURL 命令：
```bash
curl -H "Authorization: Bearer YOUR_API_TOKEN" \
     -H "Content-Type: application/json" \
     -d '[
           {
             "url": "https://www.zillow.com/homedetails/2506-Gordon-Cir-South-Bend-IN-46635/77050198_zpid/?t=for_sale"
           }
         ]' \
     "https://api.brightdata.com/datasets/v3/trigger?dataset_id=gd_lfqkr8wm13ixtbd8f5&include_errors=true"
```

### 响应示例结构
```json
{
    "property_overview": {
        "address": "73 Beverly Park Ln, Beverly Hills, CA 90210",
        "price": "$89,900,000",
        "status": "FOR_SALE",
        "living_area": "28,500 sq ft",
        "lot_size": "2.68 acres",
        "bedrooms": 9,
        "bathrooms": 22,
    },
    "key_features": {
        "highlights": [
            "85-foot infinity lap pool",
            "Two kitchens (including commercial-grade)",
            "5,000 sq ft primary suite",
            "Screening room",
            "Gated community with guard",
        ],
        "views": ["City", "Ocean", "Mountain", "Canyon"],
    },
    "financial": {
        "last_sold": "2021-04-08 for $28,500,000",
        "property_tax_rate": "1.18%",
        "monthly_hoa": "$6,216",
    },
}
```

👉 以上为部分响应。完整字段请参见[完整 JSON 响应](https://github.com/bright-cn/Zillow-Scraper/blob/main/zillow_api_data/zillow_properties.json)。

## 2. 通过筛选条件获取 Zillow 房源列表
使用位置和其他条件搜索房源。

<img width="700" alt="zillow-按输入筛选的房源列表" src="https://github.com/bright-cn/zillow-scraper/blob/main/zillow-images/zillow-properties-listing-by-input.png" />

提示：部分房源可能包含多个单元，导致返回多条记录。若需限制结果量，请使用 [Limit per input](https://docs.brightdata.com/scraping-automation/web-scraper-api/overview#limit-records)。

### 输入参数
| 参数 | 必填 | 描述 |
|------|------|------|
| `location` | 是 | 可为邮编、城市或州 |
| `listingCategory` | 是 | 选项：Sold、House for rent、House for sale |
| `HomeType` | 是 | 来自 Zillow 的户型类型（如 Houses、Apartments、Townhomes） |

### 示例请求
#### Python 代码：
```python
filters = [
    {"location": "92027", "listingCategory": "Sold", "HomeType": "Houses"},
    {"location": "New York", "listingCategory": "House for rent", "HomeType": "Condos"},
    {"location": "Colorado", "listingCategory": "", "HomeType": ""},
]
```
👉 完整 Python 脚本：[zillow_discovered_properties.py](https://github.com/bright-cn/Zillow-Scraper/blob/main/zillow_api_scraper/zillow_discovered_properties.py)

#### cURL 命令：
```bash
curl -H "Authorization: Bearer YOUR_API_TOKEN" \
     -H "Content-Type: application/json" \
     -d '[{"location": "New York", "listingCategory": "House for rent", "HomeType": "Houses"},
          {"location": "02118", "listingCategory": "House for sale", "HomeType": "Condos"},
          {"location": "Colorado", "listingCategory": "", "HomeType": ""}]' \
     "https://api.brightdata.com/datasets/v3/trigger?dataset_id=gd_lfqkr8wm13ixtbd8f5&include_errors=true&type=discover_new&discover_by=input_filters"
```

### 响应示例结构
```json
{
    "address": {
        "streetAddress": "569 Hayward Pl",
        "city": "Escondido",
        "state": "CA",
        "zipcode": "92027",
    },
    "homeStatus": "SOLD",
    "bedrooms": 4,
    "bathrooms": 2,
    "livingArea": 1446,
    "livingAreaUnits": "Square Feet",
    "lotSize": 5933,
    "lotAreaUnits": "Square Feet",
    "homeType": "SINGLE_FAMILY",
    "yearBuilt": 1987,
    "lastSoldPrice": 689000,
    "dateSoldString": "2022-08-11",
    "zestimate": 818100,
    "rentZestimate": 3752,
    "schools": [
        {
            "name": "Glen View Elementary School",
            "distance": 0.6,
            "rating": 5,
            "grades": "K-5",
        },
        {
            "name": "Hidden Valley Middle School",
            "distance": 1.2,
            "rating": 5,
            "grades": "6-8",
        },
        {
            "name": "Orange Glen High School",
            "distance": 1.4,
            "rating": 5,
            "grades": "9-12",
        },
    ],
    "url": "https://www.zillow.com/homedetails/569-Hayward-Pl-Escondido-CA-92027/16696746_zpid/",
}
```

👉 以上为部分响应。完整字段请参见[完整 JSON 响应](https://github.com/bright-cn/Zillow-Scraper/blob/main/zillow_api_data/zillow_discovered_properties.json)。

## 3. 通过 URL 获取 Zillow 房源列表
直接使用 Zillow 搜索页面的 URL 搜索房源。

<img width="700" alt="zillow-按URL的房源列表" src="https://github.com/bright-cn/zillow-scraper/blob/main/zillow-images/zillow-properties-listing-by-url.png" />

提示：部分房源可能包含多个单元，导致返回多条记录。若需限制结果量，请使用 [Limit per input](https://docs.brightdata.com/scraping-automation/web-scraper-api/overview#limit-records)。

### 输入参数
| 参数 | 必填 | 描述 |
|------|------|------|
| `url` | 是 | 包含完整搜索参数的 Zillow 搜索 URL |

### 示例请求
#### Python 代码：
```python
urls = [
    {"url": "https://www.zillow.com/south-bend-in/?searchQueryState=%7B%22pagination%22%3A..."},
    {"url": "https://www.zillow.com/new-york-ny/rentals/?searchQueryState=%7B%22isMapVisible%22%3A..."},
    {"url": "https://www.zillow.com/sands-point-ny/rentals/?searchQueryState=%7B%22isMapVisible%22%3A..."},
]
```
👉 完整 Python 脚本：[zillow_discovered_properties_by_url.py](https://github.com/bright-cn/Zillow-Scraper/blob/main/zillow_api_scraper/zillow_discovered_properties_by_url.py)

#### cURL 命令：
```bash
curl -H "Authorization: Bearer YOUR_API_TOKEN" \
     -H "Content-Type: application/json" \
     -d '[{"url": "https://www.zillow.com/south-bend-in/?searchQueryState=%7B%22pagination%22%3A..."}]' \
     "https://api.brightdata.com/datasets/v3/trigger?dataset_id=gd_lfqkr8wm13ixtbd8f5&include_errors=true&type=discover_new&discover_by=url"
```

### 响应示例结构
```json
{
    "zpid": 77029580,
    "address": {
        "streetAddress": "1937 Churchill Dr",
        "city": "South Bend",
        "state": "IN",
        "zipcode": "46617",
    },
    "price": 435000,
    "bedrooms": 4,
    "bathrooms": 4,
    "livingArea": 3197,
    "lotAreaValue": 0.46,
    "lotAreaUnits": "Acres",
    "yearBuilt": 1968,
    "homeStatus": "FOR_SALE",
    "zestimate": 420400,
    "lastSoldPrice": 134000,
    "dateSold": "2013-05-20",
    "schools": [
        {"name": "McKinley Elementary School", "rating": 4},
        {"name": "Edison Intermediate Center", "rating": 2},
        {"name": "Rise Up Academy At Eggleston", "rating": 1},
    ],
    "mortgageRates": {"thirtyYearFixedRate": 6.536},
    "listingProvidedBy": {"name": "Eric M Bomkamp", "phoneNumber": "574-360-2569"},
    "url": "https://www.zillow.com/homedetails/1937-Churchill-Dr-South-Bend-IN-46617/77029580_zpid/",
}
```
👉 以上为部分响应。完整字段请参见[完整 JSON 响应](https://github.com/bright-cn/Zillow-Scraper/blob/main/zillow_api_data/zillow_discovered_properties_by_url.json)。

## 4. Zillow 价格历史
收集某个房源的历史价格信息。

<img width="700" alt="zillow-价格历史" src="https://github.com/bright-cn/zillow-scraper/blob/main/zillow-images/zillow-price-history.png" />

### 输入参数
| 参数 | 必填 | 描述 |
|------|------|------|
| `url` | 是 | Zillow 房源 URL |

### 示例请求
#### Python 代码：
```python
urls = [
    {"url": "https://www.zillow.com/homedetails/8305-Blue-Heron-Way-Raleigh-NC-27615/6468808_zpid/"},
    {"url": "https://www.zillow.com/homedetails/930-3rd-St-SE-Hickory-NC-28602/71557289_zpid/"},
]
```
👉 完整 Python 脚本：[zillow_price_history.py](https://github.com/bright-cn/Zillow-Scraper/blob/main/zillow_api_scraper/zillow_price_history.py)

#### cURL 命令：
```bash
curl -H "Authorization: Bearer YOUR_API_TOKEN" \
     -H "Content-Type: application/json" \
     -d '[{"url": "https://www.zillow.com/homedetails/8305-Blue-Heron-Way-Raleigh-NC-27615/6468808_zpid/"},
          {"url": "https://www.zillow.com/homedetails/930-3rd-St-SE-Hickory-NC-28602/71557289_zpid/"}]' \
     "https://api.brightdata.com/datasets/v3/trigger?dataset_id=gd_lxu1cz9r88uiqsosl&include_errors=true"
```

### 响应示例结构
```json
{
    "url": "https://www.zillow.com/homedetails/8305-Blue-Heron-Way-Raleigh-NC-27615/6468808_zpid/",
    "zpid": "6468808",
    "date": "2020-11-13T00:00:00.000Z",
    "event": "Sold",
    "price": 440000,
    "price_per_squarefoot": 127,
    "source": "Doorify MLS",
    "timestamp": "2025-02-09T16:56:42.074Z",
}
```
👉 以上为部分响应。完整字段请参见[完整 JSON 响应](https://github.com/bright-cn/Zillow-Scraper/blob/main/zillow_api_data/zillow_price_history.json)。

## 无代码抓取选项
Bright Data 的无代码抓取器为无需编程即可收集 Zillow 数据提供了友好的方式。
- 几分钟即可完成抓取器配置
- 全自动化数据采集流程
- 结果可直接以多种格式下载

详细说明请参阅我们的[快速上手指南](https://github.com/bright-cn/Zillow-Scraper/blob/main/no-code-scraper.md)。

## 其他可选项
通过以下参数微调你的数据采集：

| 参数 | 类型 | 描述 | 示例 |
|------|------|------|------|
| `limit` | `integer` | 每个输入的最大返回数量 | `limit=10` |
| `include_errors` | `boolean` | 返回错误报告以便排查 | `include_errors=true` |
| `notify` | `url` | 任务完成时回调通知的 Webhook URL | `notify=https://notify-me.com/` |
| `format` | `enum` | 输出格式（如 JSON、NDJSON、JSONL、CSV） | `format=json` |

专业提示：你可以将数据传送至[外部存储](https://docs.brightdata.com/scraping-automation/web-data-apis/web-scraper-api/overview#via-deliver-to-external-storage)或[Webhook](https://docs.brightdata.com/scraping-automation/web-data-apis/web-scraper-api/overview#via-webhook)。

## 支持与资源
- API 文档：[Bright Data Docs](https://docs.brightdata.com/scraping-automation/web-scraper-api/trigger-a-collection)
- 抓取最佳实践：[避免被封锁](https://www.bright.cn/blog/web-data/web-scraping-without-getting-blocked)
- 技术支持：[联系我们](mailto:support@brightdata.com)

# warframe-drop-data 详细介绍

## 项目概述

**warframe-drop-data** 是一个专门处理 Warframe 掉落数据的项目，它将 Digital Extremes 官方掉落数据网站的信息解析成更易于使用的格式。这个项目为开发者提供了完整的掉落率信息，包括任务奖励、遗物内容、敌人掉落等关键数据。

## 主要功能

### 1. 数据解析与转换
- **官方数据解析**: 从 Digital Extremes 官方掉落数据网站获取信息
- **格式标准化**: 将原始HTML数据转换为结构化JSON
- **实时更新**: 监控官方网站变化并自动更新数据
- **数据完整性**: 确保所有掉落信息的准确性和完整性

### 2. 掉落数据类型
项目包含以下主要掉落数据类型：
- **任务奖励** (missionRewards): 各星球任务的掉落奖励
- **遗物奖励** (relics): 遗物开启的奖励内容
- **敌人掉落** (enemyModTables, enemyBlueprintTables): 敌人掉落的模组和蓝图
- **赏金任务** (cetusBountyRewards, solarisBountyRewards): 希图斯和索拉里斯联盟赏金奖励
- **突击任务** (sortieRewards): 突击任务奖励
- **虚空风暴** (keyRewards): 虚空风暴奖励
- **瞬态奖励** (transientRewards): 特殊模式奖励（如噩梦模式）
- **集团奖励** (syndicates): 集团相关奖励

### 3. Web UI 功能
- **搜索界面**: 在 http://drops.warframestat.us 提供可搜索的Web界面
- **筛选功能**: 按物品类型、掉落地点、概率等筛选
- **实时查询**: 提供实时的掉落信息查询

## 代码结构

### 核心文件
```
warframe-drop-data/
├── generateData.js          # 数据生成脚本
├── lib/                     # 核心库文件
│   ├── index.js            # 主入口
│   ├── scraper.js          # 数据抓取器
│   └── parser.js           # 数据解析器
├── data/                    # 生成的数据文件
│   ├── all.json            # 全部数据
│   ├── info.json           # 元数据信息
│   ├── missionRewards.json # 任务奖励数据
│   ├── relics.json         # 遗物数据
│   ├── missionRewards/     # 按星球分类的任务奖励
│   ├── relics/             # 按层级分类的遗物数据
│   └── ...
├── site/                    # Web UI 相关文件
├── misc/                    # 杂项文件
└── package.json            # 项目配置
```

### 数据生成流程

#### generateData.js (数据生成脚本)
- 从官方网站抓取最新数据
- 解析HTML内容并提取掉落信息
- 生成标准化的JSON数据文件
- 创建分类索引和API端点

#### 核心库组件
- **scraper.js**: 负责从官方网站抓取原始数据
- **parser.js**: 解析HTML内容并提取结构化数据
- **index.js**: 提供统一的API接口

## 数据格式

### 1. 任务奖励格式
```json
{
  "missionRewards": {
    "Earth": {
      "E Prime": {
        "gameMode": "Exterminate",
        "isEvent": false,
        "rewards": {
          "A": [
            {
              "_id": "unique_hash",
              "itemName": "Morphics",
              "chance": 3.0,
              "rarity": "Uncommon"
            }
          ],
          "B": [...],
          "C": [...]
        }
      }
    }
  }
}
```

### 2. 遗物格式
```json
{
  "relics": [
    {
      "_id": "unique_hash",
      "tier": "Axi",
      "relicName": "A1",
      "state": "Intact",
      "rewards": {
        "Intact": [
          {
            "_id": "reward_hash",
            "itemName": "Braton Prime Barrel",
            "chance": 25.33,
            "rarity": "Common"
          }
        ],
        "Exceptional": [...],
        "Flawless": [...],
        "Radiant": [...]
      }
    }
  ]
}
```

### 3. 元数据格式
```json
{
  "hash": "md5_hash_of_source_data",
  "timestamp": 1624933993968,
  "modified": 1624690737000
}
```

## API 端点

### 主要端点
- **GET /data/all.json**: 获取所有掉落数据
- **GET /data/info.json**: 获取数据元信息
- **GET /data/missionRewards.json**: 获取任务奖励数据
- **GET /data/relics.json**: 获取遗物数据
- **GET /data/sortieRewards.json**: 获取突击任务奖励
- **GET /data/transientRewards.json**: 获取瞬态奖励

### 细分端点
- **GET /data/missionRewards/[星球]/[地点].json**: 特定地点的奖励
- **GET /data/relics/[层级]/[遗物名].json**: 特定遗物的奖励

## 使用方法

### 1. 直接数据访问
```javascript
// 获取全部数据
const allData = await fetch('https://drops.warframestat.us/data/all.json')
  .then(res => res.json());

// 获取特定任务奖励
const hydronRewards = await fetch('https://drops.warframestat.us/data/missionRewards/Sedna/Hydron.json')
  .then(res => res.json());
```

### 2. 本地使用
```javascript
// 作为Node.js模块使用
const dropData = require('./lib/index.js');

// 生成数据
const generateData = require('./generateData.js');
generateData();
```

### 3. Web界面使用
访问 http://drops.warframestat.us 使用可视化界面：
- 搜索特定物品的掉落地点
- 查看任务奖励轮换
- 比较不同地点的掉落率

## 技术细节

### 数据源
- **官方网站**: https://warframe-web-assets.nyc3.cdn.digitaloceanspaces.com/uploads/cms/hnfvc0o3jnfvc873njb03enrf56.html
- **更新频率**: 跟踪官方网站修改时间戳
- **数据验证**: 通过MD5哈希验证数据完整性

### 依赖关系
- **cheerio**: HTML解析
- **node-fetch**: HTTP请求
- **express**: Web服务器
- **crypto**: 哈希计算
- **fs**: 文件系统操作

### 数据处理流程
1. **抓取**: 从官方网站获取HTML内容
2. **解析**: 提取表格数据和掉落信息
3. **标准化**: 转换为统一的JSON格式
4. **验证**: 检查数据完整性和准确性
5. **部署**: 生成API端点和Web界面

## 数据准确性

### 官方数据源
- 直接从 Digital Extremes 官方掉落数据网站获取
- 无数据挖掘，完全基于公开信息
- 实时监控官方更新

### 数据验证
- MD5哈希验证数据完整性
- 时间戳跟踪数据新鲜度
- 自动化测试确保解析正确性

## 性能考虑

### 缓存策略
- 增量更新机制
- 文件级别的缓存
- CDN分发支持

### 数据压缩
- 提供slim版本 (all.slim.json)
- 按需加载特定数据
- 分类存储减少传输量

## 部署与维护

### 自动化部署
- GitHub Actions自动构建
- Heroku部署支持
- 定时任务更新数据

### 监控与日志
- 数据更新监控
- 错误日志记录
- 性能指标跟踪

## 开发者信息

- **维护者**: WFCD (Warframe Community Developers)
- **许可证**: MIT
- **仓库**: https://github.com/WFCD/warframe-drop-data
- **Web界面**: http://drops.warframestat.us
- **数据源**: Digital Extremes官方掉落数据

## 社区支持

- **Discord**: https://discord.gg/jGZxH9f
- **问题报告**: GitHub Issues
- **贡献指南**: 欢迎社区贡献
- **API支持**: 通过WFCD社区提供技术支持

## 使用案例

### 开发者集成
- 游戏助手应用
- 掉落率计算器
- 刷怪效率分析工具

### 数据分析
- 掉落概率统计
- 刷怪地点推荐
- 物品获取难度评估 
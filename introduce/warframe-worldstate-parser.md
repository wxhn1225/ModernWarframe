# warframe-worldstate-parser 详细介绍

## 项目概述

**warframe-worldstate-parser** 是一个专门解析 Warframe 世界状态数据的JavaScript库。它将从官方API获取的原始世界状态数据转换为易于使用的JavaScript对象，为开发者提供了访问游戏实时信息的便捷方式。

## 主要功能

### 1. 世界状态数据解析
- **原始数据转换**: 将官方API的原始JSON数据转换为结构化对象
- **数据标准化**: 统一不同数据源的格式和结构
- **实时信息**: 提供警报、入侵、裂缝等实时游戏信息
- **多语言支持**: 支持多种语言的本地化数据

### 2. 解析的数据类型
项目解析以下主要的世界状态数据：
- **警报** (Alerts): 限时警报任务
- **入侵** (Invasions): 派系入侵事件
- **裂缝** (Fissures): 虚空裂缝任务
- **突击任务** (Sorties): 每日突击任务
- **仲裁** (Arbitrations): 仲裁任务
- **夜灵电波** (Nightwave): 夜灵电波活动
- **地球周期** (Earth Cycle): 地球昼夜周期
- **希图斯周期** (Cetus Cycle): 希图斯昼夜周期
- **瓦利斯周期** (Vallis Cycle): 瓦利斯温度周期
- **虚空商人** (Void Trader): 虚空商人巴洛基尔
- **新闻** (News): 游戏内新闻
- **事件** (Events): 特殊事件
- **建造进度** (Construction Progress): 建造进度
- **太阳系信息** (Solar System): 太阳系状态

### 3. 数据增强
- **时间转换**: 将游戏时间转换为现实时间
- **本地化**: 支持多语言翻译
- **数据验证**: 验证数据完整性和有效性
- **格式化**: 提供易读的数据格式

## 代码结构

### 核心文件
```
warframe-worldstate-parser/
├── lib/                     # 核心库文件
│   ├── index.js            # 主入口文件
│   ├── Alert.js            # 警报解析
│   ├── Invasion.js         # 入侵解析
│   ├── Fissure.js          # 裂缝解析
│   ├── Sortie.js           # 突击任务解析
│   ├── Arbitration.js      # 仲裁解析
│   ├── Nightwave.js        # 夜灵电波解析
│   ├── News.js             # 新闻解析
│   ├── Event.js            # 事件解析
│   ├── VoidTrader.js       # 虚空商人解析
│   ├── CetusCycle.js       # 希图斯周期解析
│   ├── VallisCycle.js      # 瓦利斯周期解析
│   ├── EarthCycle.js       # 地球周期解析
│   └── ...
├── resources/               # 资源文件
│   ├── locales/            # 本地化文件
│   └── ...
├── test/                    # 测试文件
├── main.js                  # 主文件
├── package.json            # 项目配置
└── tsconfig.json           # TypeScript配置
```

### 解析器类结构

#### 基础解析器
每个解析器都继承自基础类，提供统一的接口：
```javascript
class BaseParser {
  constructor(data, locale) {
    this.data = data;
    this.locale = locale;
  }
  
  toString() {
    // 返回格式化的字符串表示
  }
}
```

#### 特定解析器
- **Alert.js**: 解析警报任务，包括奖励、任务类型、剩余时间等
- **Invasion.js**: 解析入侵事件，包括派系、进度、奖励等
- **Fissure.js**: 解析裂缝任务，包括遗物层级、任务类型、剩余时间等
- **Sortie.js**: 解析突击任务，包括变体、boss、奖励等

## 使用方法

### 1. 基本使用
```javascript
import WorldStateParser from 'warframe-worldstate-parser';

// 获取世界状态数据
const worldstateData = await fetch('https://content.warframe.com/dynamic/worldState.php')
  .then(data => data.text());

// 解析数据
const ws = await WorldStateParser(worldstateData);

// 访问解析后的数据
console.log(ws.alerts[0].toString());
console.log(ws.invasions.length);
console.log(ws.fissures.filter(f => f.tier === 'Axi'));
```

### 2. 多语言支持
```javascript
// 指定语言
const ws = await WorldStateParser(worldstateData, 'zh');

// 获取中文格式的数据
console.log(ws.alerts[0].toString());
```

### 3. 特定数据访问
```javascript
// 访问特定类型的数据
const activeAlerts = ws.alerts.filter(alert => !alert.expired);
const currentInvasions = ws.invasions.filter(inv => !inv.completed);
const availableFissures = ws.fissures.filter(fissure => !fissure.expired);

// 获取周期信息
console.log(`地球: ${ws.earthCycle.state} (${ws.earthCycle.timeLeft})`);
console.log(`希图斯: ${ws.cetusCycle.state} (${ws.cetusCycle.timeLeft})`);
```

## 数据格式

### 1. 警报格式
```javascript
{
  id: 'unique_alert_id',
  activation: Date,
  expiry: Date,
  mission: {
    node: 'mission_node',
    type: 'mission_type',
    faction: 'enemy_faction',
    reward: {
      items: [...],
      credits: number
    }
  },
  expired: boolean,
  eta: 'time_remaining'
}
```

### 2. 入侵格式
```javascript
{
  id: 'unique_invasion_id',
  node: 'mission_node',
  desc: 'invasion_description',
  attackerReward: {...},
  defenderReward: {...},
  attackingFaction: 'faction_name',
  defendingFaction: 'faction_name',
  vsInfestation: boolean,
  completion: number,
  completed: boolean,
  eta: 'time_remaining'
}
```

### 3. 裂缝格式
```javascript
{
  id: 'unique_fissure_id',
  node: 'mission_node',
  missionType: 'mission_type',
  enemy: 'enemy_faction',
  tier: 'relic_tier',
  tierNum: number,
  activation: Date,
  expiry: Date,
  expired: boolean,
  eta: 'time_remaining'
}
```

## 技术细节

### 依赖关系
- **warframe-worldstate-data**: 提供本地化数据和映射
- **node-fetch**: HTTP请求处理
- **moment**: 时间处理和格式化
- **util**: Node.js 实用工具

### 数据处理流程
1. **原始数据获取**: 从官方API获取JSON数据
2. **数据验证**: 检查数据格式和完整性
3. **解析处理**: 使用特定解析器处理不同类型数据
4. **本地化**: 应用语言本地化
5. **格式化**: 生成最终的对象结构

### 时间处理
- **游戏时间转换**: 将游戏内时间转换为现实时间
- **时区处理**: 支持不同时区的时间显示
- **剩余时间计算**: 计算事件剩余时间

### 本地化支持
- **多语言**: 支持多种语言的本地化
- **动态加载**: 根据需要加载特定语言包
- **Crowdin集成**: 通过Crowdin进行翻译管理

## 性能优化

### 缓存策略
- **数据缓存**: 缓存解析结果减少重复计算
- **增量更新**: 只更新变化的数据
- **内存管理**: 优化内存使用

### 错误处理
- **数据验证**: 验证输入数据的有效性
- **异常捕获**: 捕获和处理解析异常
- **降级处理**: 在数据不完整时提供默认值

## 实时API服务

### 官方API端点
- **PC**: https://api.warframestat.us/pc
- **PS4**: https://api.warframestat.us/ps4
- **Xbox**: https://api.warframestat.us/xb1
- **Switch**: https://api.warframestat.us/swi

### 使用示例
```javascript
// 获取PC平台的世界状态
const pcWorldState = await fetch('https://api.warframestat.us/pc')
  .then(res => res.json());

// 获取特定数据
const alerts = await fetch('https://api.warframestat.us/pc/alerts')
  .then(res => res.json());
```

## 测试与质量保证

### 测试覆盖
- **单元测试**: 测试每个解析器的功能
- **集成测试**: 测试完整的解析流程
- **回归测试**: 确保版本兼容性

### 代码质量
- **ESLint**: 代码风格检查
- **TypeScript**: 类型检查
- **覆盖率测试**: 确保测试覆盖率

## 开发者信息

- **维护者**: WFCD (Warframe Community Developers)
- **许可证**: MIT
- **仓库**: https://github.com/WFCD/warframe-worldstate-parser
- **NPM**: warframe-worldstate-parser
- **文档**: https://wfcd.github.io/warframe-worldstate-parser/

## 社区支持

- **Discord**: https://discord.gg/jGZxH9f
- **问题报告**: GitHub Issues
- **贡献指南**: 欢迎社区贡献
- **翻译**: 通过Crowdin参与翻译

## 使用案例

### 开发者应用
- **游戏助手**: 显示实时游戏信息
- **通知系统**: 基于游戏事件的通知
- **数据分析**: 游戏模式和事件分析

### 社区工具
- **Discord Bot**: 游戏状态通知机器人
- **Web应用**: 游戏状态查看网站
- **移动应用**: 手机端游戏助手

## 扩展与定制

### 自定义解析器
```javascript
// 创建自定义解析器
class CustomParser extends BaseParser {
  constructor(data, locale) {
    super(data, locale);
    // 自定义逻辑
  }
}

// 扩展解析器
const customWS = new WorldStateParser(data, locale, {
  customParser: CustomParser
});
```

### 插件系统
- **解析器插件**: 添加新的数据类型解析
- **格式化插件**: 自定义输出格式
- **本地化插件**: 添加新的语言支持 
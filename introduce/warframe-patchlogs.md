# warframe-patchlogs 详细介绍

## 项目概述

**warframe-patchlogs** 是一个专门处理 Warframe 补丁日志的项目，它将官方论坛上的补丁笔记解析成结构化的JSON数据。这个项目主要为 [warframe-items](https://github.com/WFCD/warframe-items) 项目提供补丁历史数据支持，同时也可以独立使用来查询特定物品的更新历史。

## 主要功能

### 1. 补丁日志解析
- **论坛数据抓取**: 从官方 Warframe 论坛抓取补丁笔记
- **HTML解析**: 将HTML格式的补丁笔记转换为结构化数据
- **数据标准化**: 统一不同版本补丁笔记的格式
- **历史追踪**: 维护完整的补丁历史记录

### 2. 物品变更追踪
- **物品关联**: 将补丁变更与特定物品关联
- **变更分类**: 按照添加、修改、修复等类型分类变更
- **历史查询**: 查询特定物品的所有历史变更
- **影响分析**: 分析补丁对物品的影响

### 3. 数据提供
- **JSON格式**: 提供标准化的JSON格式数据
- **API接口**: 提供编程接口访问补丁数据
- **搜索功能**: 支持按物品名称和类型搜索
- **时间范围**: 支持按时间范围查询补丁

## 代码结构

### 核心文件
```
warframe-patchlogs/
├── index.js               # 主入口文件
├── index.d.ts             # TypeScript类型定义
├── package.json           # 项目配置
├── data/                  # 数据存储目录
│   ├── patchlogs.json     # 完整补丁日志数据
│   ├── cache/             # 缓存文件
│   └── ...
├── build/                 # 构建脚本
│   ├── scraper.js         # 论坛数据抓取器
│   ├── parser.js          # HTML解析器
│   ├── processor.js       # 数据处理器
│   └── generator.js       # 数据生成器
├── test/                  # 测试文件
└── ...
```

### 数据处理流程

#### 1. 数据抓取 (scraper.js)
- 从官方论坛获取补丁笔记列表
- 下载每个补丁笔记的HTML内容
- 处理分页和链接跳转
- 管理请求频率和缓存

#### 2. 内容解析 (parser.js)
- 解析HTML结构提取补丁信息
- 识别补丁类型（正式版、热修复等）
- 提取发布时间和版本号
- 分离不同类型的变更内容

#### 3. 数据处理 (processor.js)
- 清理和标准化文本内容
- 识别物品名称和相关信息
- 分类变更类型（添加、修改、修复）
- 建立物品与补丁的关联关系

#### 4. 数据生成 (generator.js)
- 生成标准化的JSON格式数据
- 创建索引和查询优化结构
- 生成API响应格式
- 输出最终数据文件

## 数据格式

### 1. 补丁日志对象
```javascript
{
  name: 'Beasts of the Sanctuary: Hotfix 22.20.6',
  url: 'https://forums.warframe.com/topic/960140-beasts-of-the-sanctuary-hotfix-22206/',
  date: '2018-05-24T22:00:50Z',
  imgUrl: 'https://i.imgur.com/6ymAONO.jpg',
  description: 'The Orokin Decoration costs/refunds mentioned in Hotfix 22.20.3...',
  additions: 'New features and content added...',
  changes: 'Balance changes and modifications...',
  fixes: 'Bug fixes and corrections...',
  type: 'Hotfix'
}
```

### 2. 物品变更记录
```javascript
{
  item: 'Ash Prime',
  type: 'Warframe',
  changes: [
    {
      patch: 'Update 22.20.0',
      date: '2018-05-17T19:00:00Z',
      type: 'change',
      description: 'Increased base health from 100 to 125'
    },
    {
      patch: 'Hotfix 22.20.6',
      date: '2018-05-24T22:00:50Z',
      type: 'fix',
      description: 'Fixed ability animation issue'
    }
  ]
}
```

### 3. 补丁统计信息
```javascript
{
  totalPatches: 1205,
  dateRange: {
    earliest: '2013-03-25T00:00:00Z',
    latest: '2024-01-15T20:30:00Z'
  },
  types: {
    'Update': 45,
    'Hotfix': 892,
    'Prime Resurgence': 12,
    'Nightwave': 8
  }
}
```

## 使用方法

### 1. 基本使用
```javascript
const patchlogs = require('warframe-patchlogs');

// 获取所有补丁日志
for (let post of patchlogs.posts) {
  console.log(`${post.name}: ${post.date}`);
}

// 查询特定物品的变更历史
const ashPrimeChanges = patchlogs.getItemChanges({ 
  name: 'Ash Prime', 
  type: 'Warframe' 
});

console.log(ashPrimeChanges);
```

### 2. 高级查询
```javascript
// 按时间范围查询
const recentPatches = patchlogs.posts.filter(post => {
  const patchDate = new Date(post.date);
  const lastMonth = new Date();
  lastMonth.setMonth(lastMonth.getMonth() - 1);
  return patchDate > lastMonth;
});

// 按补丁类型查询
const hotfixes = patchlogs.posts.filter(post => post.type === 'Hotfix');
const updates = patchlogs.posts.filter(post => post.type === 'Update');
```

### 3. 物品变更分析
```javascript
// 获取最活跃的物品（变更次数最多）
const getItemChangeCount = (itemName, itemType) => {
  const changes = patchlogs.getItemChanges({ name: itemName, type: itemType });
  return changes.length;
};

// 分析补丁内容
const analyzePatches = (patches) => {
  const analysis = {
    totalFixes: 0,
    totalChanges: 0,
    totalAdditions: 0
  };
  
  patches.forEach(patch => {
    if (patch.fixes) analysis.totalFixes++;
    if (patch.changes) analysis.totalChanges++;
    if (patch.additions) analysis.totalAdditions++;
  });
  
  return analysis;
};
```

## 技术细节

### 数据源
- **官方论坛**: https://forums.warframe.com/forum/3-pc-update-build-notes/
- **更新检测**: 监控论坛新帖子和更新
- **缓存机制**: 避免重复抓取相同内容

### 依赖关系
- **cheerio**: HTML解析和DOM操作
- **axios/node-fetch**: HTTP请求处理
- **moment**: 时间处理和格式化
- **fs**: 文件系统操作
- **path**: 路径处理

### 数据处理算法
```javascript
// 物品名称识别算法
const identifyItems = (patchText) => {
  const itemPatterns = [
    /\b([A-Z][a-z]+(?:\s+[A-Z][a-z]+)*)\s+(?:Prime|Vandal|Wraith|Prisma)\b/g,
    /\b([A-Z][a-z]+(?:\s+[A-Z][a-z]+)*)\s+(?:Blueprint|Parts)\b/g,
    // 更多模式...
  ];
  
  const items = [];
  itemPatterns.forEach(pattern => {
    const matches = patchText.match(pattern);
    if (matches) items.push(...matches);
  });
  
  return items;
};

// 变更类型分类
const classifyChange = (changeText) => {
  const changeKeywords = {
    'fix': ['fixed', 'corrected', 'resolved'],
    'nerf': ['reduced', 'decreased', 'nerfed'],
    'buff': ['increased', 'improved', 'enhanced'],
    'addition': ['added', 'introduced', 'new']
  };
  
  // 分类逻辑...
};
```

## 数据质量保证

### 解析准确性
- **多重验证**: 使用多种方式验证解析结果
- **人工校验**: 关键数据进行人工验证
- **社区反馈**: 接受社区反馈改进解析准确性

### 数据完整性
- **历史完整性**: 确保历史数据的完整性
- **关联验证**: 验证物品与补丁的关联关系
- **时间戳准确性**: 确保时间信息的准确性

### 错误处理
```javascript
// 解析错误处理
const safeParseHTML = (html) => {
  try {
    const $ = cheerio.load(html);
    return extractPatchData($);
  } catch (error) {
    console.error('HTML解析错误:', error);
    return null;
  }
};

// 数据验证
const validatePatchData = (patchData) => {
  const required = ['name', 'url', 'date'];
  return required.every(field => patchData[field]);
};
```

## 性能优化

### 缓存策略
- **增量更新**: 只处理新的补丁笔记
- **本地缓存**: 缓存已处理的数据
- **压缩存储**: 使用压缩格式存储大量数据

### 内存管理
- **流式处理**: 使用流式处理大量数据
- **垃圾回收**: 及时清理不用的数据
- **分批处理**: 分批处理避免内存溢出

## 集成与应用

### warframe-items集成
```javascript
// 在warframe-items中的使用
const patchlogs = require('warframe-patchlogs');

// 为每个物品添加补丁历史
items.forEach(item => {
  item.patchlogs = patchlogs.getItemChanges({
    name: item.name,
    type: item.category
  });
});
```

### 独立应用
- **补丁查看器**: 浏览和搜索补丁历史
- **物品跟踪**: 跟踪特定物品的变更
- **数据分析**: 分析游戏平衡性变化

## 更新与维护

### 自动化更新
- **定时任务**: 定期检查新的补丁笔记
- **增量处理**: 只处理新增的内容
- **版本控制**: 维护数据版本历史

### 监控与报警
- **解析失败检测**: 检测解析错误
- **数据异常监控**: 监控数据质量
- **更新通知**: 新补丁数据可用通知

## 开发者信息

- **维护者**: WFCD (Warframe Community Developers)
- **许可证**: MIT
- **仓库**: https://github.com/WFCD/warframe-patchlogs
- **NPM**: warframe-patchlogs
- **版本**: 当前版本 2.78.0

## 社区支持

- **Discord**: https://discord.gg/jGZxH9f
- **问题报告**: GitHub Issues
- **贡献指南**: 欢迎社区贡献解析规则改进
- **数据校验**: 社区帮助验证数据准确性

## 使用案例

### 开发者工具
- **游戏助手**: 显示物品变更历史
- **平衡分析**: 分析游戏平衡性变化
- **趋势分析**: 分析开发者的调整趋势

### 社区应用
- **补丁追踪**: 跟踪感兴趣的补丁内容
- **物品历史**: 查看物品的演变历史
- **版本对比**: 比较不同版本的差异

## 未来发展

### 功能扩展
- **多语言支持**: 支持其他语言的补丁笔记
- **智能分析**: 使用AI分析补丁内容
- **预测模型**: 基于历史数据预测未来变更

### 数据增强
- **详细分类**: 更详细的变更类型分类
- **影响评估**: 评估补丁对游戏的影响
- **社区反馈**: 整合社区对补丁的反馈

这个项目在 Warframe 生态系统中扮演着重要的历史记录角色，为开发者和玩家提供了宝贵的变更追踪数据。 
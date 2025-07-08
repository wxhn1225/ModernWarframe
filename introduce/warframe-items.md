# warframe-items 详细介绍

## 项目概述

**warframe-items** 是一个全面的 Warframe 物品数据库项目，它从 Warframe 的官方 API 获取所有游戏内物品的详细信息。这个项目被设计为 Warframe 社区开发者的核心数据源，提供了最完整和最准确的物品数据。

## 主要功能

### 1. 数据获取与整合
- **API 数据获取**: 从 Warframe 官方移动端 API 获取物品数据
- **图像处理**: 自动下载、压缩和优化所有物品图像
- **掉落率数据**: 集成掉落率信息
- **补丁日志**: 为每个物品提供相关的补丁历史
- **Riven 数据**: 包含相关的 Riven 模组信息

### 2. 数据分类
项目提供了14个主要的物品分类：
- **All**: 所有物品的合集
- **Arcanes**: 秘奥、Zaw 和部分 Warframe 秘奥
- **Archwing**: 天使之翼装备
- **Arch-Gun**: 天使之翼武器
- **Arch-Melee**: 天使之翼近战武器
- **Enemy**: NPC 敌人数据
- **Fish**: 可捕获的鱼类
- **Gear**: 装备轮盘道具
- **Glyphs**: 合作伙伴徽章等
- **Melee**: 近战武器
- **Misc**: 未分类物品
- **Mods**: 武器、Warframe、天使之翼等模组
- **Node**: 任务节点
- **Pets**: 宠物伙伴
- **Primary**: 主武器
- **Quests**: 任务
- **Relics**: 遗物
- **Resources**: 建造材料
- **Secondary**: 副武器
- **Sentinels**: 机械伙伴
- **SentinelWeapons**: 机械伙伴专用武器
- **Sigils**: 胸部和背部装饰
- **Skins**: 皮肤
- **Warframes**: 战甲

### 3. 数据特色
- **唯一游戏内名称**: 提供完整的游戏内路径名 (如 /Lotus/Weapons/Tenno/...)
- **掉落率信息**: 详细的掉落概率数据
- **补丁日志**: 每个物品的更新历史
- **压缩图像**: 优化后的物品图像文件
- **Riven 兼容性**: Riven 模组相关数据
- **交易状态**: 物品是否可交易

## 代码结构

### 核心文件
```
warframe-items/
├── index.js/mjs          # 主入口文件，支持 CommonJS 和 ESM
├── index.d.ts            # TypeScript 类型定义
├── package.json          # 项目配置和依赖
├── data/                 # 数据存储目录
│   ├── json/            # JSON 格式的物品数据
│   ├── img/             # 物品图像文件
│   └── cache/           # 构建缓存
├── build/               # 构建脚本
│   ├── build.mjs        # 主构建脚本
│   ├── parser.mjs       # 数据解析器
│   ├── scraper.mjs      # 数据抓取器
│   └── ...
├── utilities/           # 工具函数
│   ├── find.mjs         # 查找功能
│   ├── colors.mjs       # 颜色工具
│   └── Pixel.mjs        # 像素处理
└── test/                # 测试文件
```

### 构建系统

#### build.mjs (主构建脚本)
- 协调整个构建过程
- 管理JSON数据输出
- 处理图像压缩和缓存
- 验证数据完整性

#### parser.mjs (数据解析器)
- 解析原始API数据
- 标准化数据格式
- 处理特殊字段和关系
- 生成类型化数据结构

#### scraper.mjs (数据抓取器)
- 从多个数据源获取信息
- 处理网络请求和重试
- 管理API限制和缓存
- 集成外部数据源

#### 工具函数
- **find.mjs**: 提供强大的物品查找功能
- **colors.mjs**: 颜色处理和主题色提取
- **Pixel.mjs**: 图像像素操作和处理

## 使用方法

### 1. 基本使用
```javascript
// CommonJS
const Items = require('@wfcd/items');
const items = new Items();

// ESM
import Items from '@wfcd/items';
const items = new Items();

// 获取所有物品
console.log(items.length);

// 使用工具函数
import { find, colors } from '@wfcd/items/utilities';
const excalPrime = find.findItem('/Lotus/Powersuits/Excalibur/ExcaliburPrime');
```

### 2. 选择性加载
```javascript
// 只加载特定分类以节省内存
const items = new Items({ category: ['Primary', 'Secondary'] });

// 添加自定义物品
const items = new Items(options, ...customItems);
```

### 3. 数据访问
```javascript
// 直接访问预编译数据
// 位置: /data/json/[Category].json
// 图像: /data/img/
// 通过CDN访问: https://cdn.warframestat.us/img/${item.imageName}
```

## 技术细节

### 依赖关系
- **核心依赖**: 无运行时依赖
- **对等依赖**: warframe-worldstate-data
- **构建依赖**: 
  - 图像处理: sharp, imagemin
  - 数据处理: cheerio, lodash
  - 网络: node-fetch, https-proxy-agent
  - 压缩: lzma
  - 脚本: lua.vm.js

### 数据格式
每个物品包含以下基本结构：
```typescript
interface Item {
  uniqueName: string;          // 游戏内唯一标识
  name: string;                // 显示名称
  description?: string;        // 描述
  type: string;                // 物品类型
  category: string;            // 分类
  tradable: boolean;           // 是否可交易
  patchlogs?: PatchLog[];      // 补丁历史
  imageName?: string;          // 图像文件名
  // 其他特定属性...
}
```

### 构建过程
1. **数据抓取**: 从官方API获取最新数据
2. **数据解析**: 标准化和清理原始数据
3. **图像处理**: 下载、压缩和优化图像
4. **数据验证**: 确保数据完整性和类型正确性
5. **文件生成**: 生成分类JSON文件和汇总文件

### 自动化更新
- 项目会在每次游戏更新时自动重建
- 监控掉落率变化和图像更新
- 自动发布到NPM

## 数据准确性

### 数据源
- **官方API**: 直接从 Warframe 官方数据源获取
- **实时更新**: 跟踪游戏版本变化
- **社区验证**: 通过 WFCD 社区验证数据准确性

### 质量保证
- 自动化测试覆盖核心功能
- TypeScript 类型检查
- 数据完整性验证
- 回归测试确保向后兼容性

## 性能考虑

### 内存优化
- 分类加载减少内存使用
- 图像压缩减少存储空间
- 缓存机制提高构建效率

### 网络优化
- CDN 支持的图像服务
- 压缩的JSON数据格式
- 增量更新机制

## 开发者信息

- **维护者**: WFCD (Warframe Community Developers)
- **许可证**: MIT
- **仓库**: https://github.com/WFCD/warframe-items
- **NPM**: @wfcd/items
- **文档**: https://github.com/WFCD/warframe-items#readme

## 社区支持

- **Discord**: https://discord.gg/jGZxH9f
- **问题报告**: GitHub Issues
- **贡献指南**: CONTRIBUTING.md
- **API支持**: 通过 WFCD 社区提供 
# warframe-worldstate-data 详细介绍

## 项目概述

**warframe-worldstate-data** 是一个专门为 warframe-worldstate-parser 提供数据支持的库。它包含了解析 Warframe 世界状态数据所需的所有映射、翻译和配置信息，为世界状态解析器提供了必要的数据基础。

## 主要功能

### 1. 数据映射服务
- **ID映射**: 将游戏内部ID映射到可读的名称
- **节点映射**: 太阳系节点信息和任务类型映射
- **派系映射**: 派系信息和关联数据
- **物品映射**: 物品ID到名称的映射关系

### 2. 本地化支持
- **多语言翻译**: 支持多种语言的本地化数据
- **动态加载**: 根据需要加载特定语言包
- **翻译管理**: 通过Crowdin进行翻译管理
- **字符串模板**: 支持参数化的翻译字符串

### 3. 游戏数据结构
- **太阳系节点**: 完整的太阳系节点信息
- **任务类型**: 各种任务类型的定义
- **派系数据**: 派系信息和关联数据
- **集团数据**: 集团信息和奖励系统
- **操作类型**: 全局修改器的操作类型

## 代码结构

### 核心文件
```
warframe-worldstate-data/
├── data/                    # 数据文件目录
│   ├── conclaveData.json   # 联盟数据
│   ├── eventsData.json     # 事件数据
│   ├── factionsData.json   # 派系数据
│   ├── fissureModifiers.json # 裂缝修饰符
│   ├── languages.json      # 语言字符串
│   ├── missionTypes.json   # 任务类型
│   ├── operationTypes.json # 操作类型
│   ├── persistentEnemyData.json # 持续敌人数据
│   ├── solNodes.json       # 太阳系节点
│   ├── syndicatesData.json # 集团数据
│   ├── upgradeTypes.json   # 升级类型
│   └── [lang]/             # 各语言本地化文件
│       ├── en/             # 英语
│       ├── zh/             # 中文
│       ├── ja/             # 日语
│       └── ...
├── exports.ts              # 导出文件
├── types.ts                # 类型定义
├── safeImport.ts           # 安全导入工具
├── tools/                  # 工具目录
├── test/                   # 测试文件
└── package.json           # 项目配置
```

### 数据访问器

#### exports.ts (主导出文件)
```typescript
// 导出所有数据访问器
export const conclave = conclaveData;
export const events = eventsData;
export const factions = factionsData;
export const fissureModifiers = fissureModifiersData;
export const languages = languagesData;
export const missionTypes = missionTypesData;
export const operationTypes = operationTypesData;
export const persistentEnemy = persistentEnemyData;
export const solNodes = solNodesData;
export const syndicates = syndicatesData;
export const upgradeTypes = upgradeTypesData;
```

#### types.ts (类型定义)
```typescript
// 定义数据结构类型
export interface SolNode {
  value: string;
  type: string;
  enemy: string;
  name?: string;
}

export interface MissionType {
  key: string;
  value: string;
}

export interface FactionData {
  key: string;
  value: string;
}
```

## 数据格式

### 1. 太阳系节点 (solNodes.json)
```json
{
  "SolNode1": {
    "value": "Galatea (Neptune)",
    "type": "MT_CAPTURE",
    "enemy": "FC_CORPUS",
    "name": "Galatea"
  },
  "SolNode2": {
    "value": "Lith (Earth)",
    "type": "MT_EXTERMINATE",
    "enemy": "FC_GRINEER",
    "name": "Lith"
  }
}
```

### 2. 任务类型 (missionTypes.json)
```json
{
  "MT_EXTERMINATE": "Exterminate",
  "MT_SURVIVAL": "Survival",
  "MT_DEFENSE": "Defense",
  "MT_MOBILE_DEFENSE": "Mobile Defense",
  "MT_CAPTURE": "Capture",
  "MT_RESCUE": "Rescue",
  "MT_SABOTAGE": "Sabotage",
  "MT_SPY": "Spy",
  "MT_EXCAVATE": "Excavation",
  "MT_TERRITORY": "Interception"
}
```

### 3. 派系数据 (factionsData.json)
```json
{
  "FC_GRINEER": "Grineer",
  "FC_CORPUS": "Corpus",
  "FC_INFESTATION": "Infested",
  "FC_CORRUPTED": "Corrupted",
  "FC_OROKIN": "Orokin",
  "FC_TENNO": "Tenno"
}
```

### 4. 语言字符串 (languages.json)
```json
{
  "/Lotus/Language/Items/SwordAndShieldWeaponName": "Sword & Shield",
  "/Lotus/Language/Menu/Profile_TotalCredits": "Total Credits",
  "/Lotus/Language/Sorties/SortieRewardSkulls": "Ayatan Anasa Sculpture",
  "/Lotus/Language/Game/MissionComplete": "Mission Complete"
}
```

### 5. 集团数据 (syndicatesData.json)
```json
{
  "ArbitersSyndicate": {
    "name": "The Arbiters of Hexis",
    "key": "ArbitersSyndicate"
  },
  "CephalonSudaSyndicate": {
    "name": "Cephalon Suda",
    "key": "CephalonSudaSyndicate"
  },
  "NewLokaSyndicate": {
    "name": "New Loka",
    "key": "NewLokaSyndicate"
  }
}
```

## 使用方法

### 1. 基本使用
```javascript
import worldstateData from 'warframe-worldstate-data';

// 获取太阳系节点数据
const nodes = worldstateData.solNodes;
const erpo = nodes['SolNode903'];
const { enemy, value, type } = erpo;

// 获取任务类型
const missionTypes = worldstateData.missionTypes;
const exterminateType = missionTypes['MT_EXTERMINATE'];

// 获取派系信息
const factions = worldstateData.factions;
const grineerName = factions['FC_GRINEER'];
```

### 2. 工具函数使用
```javascript
import utilities from 'warframe-worldstate-data/utilities';
const { getLanguage } = utilities;

// 获取特定语言的数据
const chineseData = getLanguage('zh');
```

### 3. 类型支持
```typescript
import { SolNode, MissionType } from 'warframe-worldstate-data/types';

// 使用类型定义
const node: SolNode = worldstateData.solNodes['SolNode1'];
const mission: MissionType = worldstateData.missionTypes['MT_EXTERMINATE'];
```

## 技术细节

### 依赖关系
- **核心**: 无运行时依赖
- **开发依赖**: 
  - TypeScript: 类型检查
  - Mocha: 测试框架
  - Biome: 代码格式化
  - Crowdin: 翻译管理

### 数据生成流程
1. **数据收集**: 从游戏文件和API收集原始数据
2. **数据清理**: 清理和标准化数据格式
3. **映射生成**: 创建ID到名称的映射关系
4. **翻译集成**: 整合多语言翻译
5. **类型生成**: 生成TypeScript类型定义

### 本地化管理
- **Crowdin集成**: 通过Crowdin管理翻译
- **自动化更新**: 自动拉取最新翻译
- **质量检查**: 验证翻译完整性和准确性

## 数据维护

### 更新流程
1. **游戏更新监控**: 监控游戏内容更新
2. **数据同步**: 同步新的游戏数据
3. **映射更新**: 更新ID映射关系
4. **翻译更新**: 更新多语言翻译
5. **版本发布**: 发布新版本

### 质量保证
- **自动化测试**: 测试数据完整性
- **类型检查**: TypeScript类型验证
- **数据验证**: 检查数据格式和关联性

## 性能优化

### 数据加载
- **按需加载**: 根据需要加载特定数据
- **缓存机制**: 缓存常用数据
- **压缩优化**: 压缩数据文件减少大小

### 内存管理
- **懒加载**: 延迟加载不常用数据
- **数据共享**: 共享相同的数据实例
- **垃圾回收**: 及时清理不用的数据

## 数据准确性

### 数据源
- **官方数据**: 基于官方游戏数据
- **社区验证**: 通过社区验证数据准确性
- **持续更新**: 跟踪游戏更新保持数据最新

### 验证机制
- **数据校验**: 验证数据格式和完整性
- **关联检查**: 检查数据间的关联关系
- **测试覆盖**: 全面的测试覆盖

## 扩展性

### 新数据类型
- **模块化设计**: 支持添加新的数据类型
- **标准化接口**: 统一的数据访问接口
- **类型扩展**: 支持TypeScript类型扩展

### 自定义数据
- **数据覆盖**: 支持自定义数据覆盖
- **插件系统**: 支持数据插件扩展
- **配置化**: 支持配置化的数据管理

## 开发者信息

- **维护者**: WFCD (Warframe Community Developers)
- **许可证**: MIT
- **仓库**: https://github.com/WFCD/warframe-worldstate-data
- **NPM**: warframe-worldstate-data
- **文档**: README.md

## 社区支持

- **Discord**: https://discord.gg/jGZxH9f
- **问题报告**: GitHub Issues
- **贡献指南**: 欢迎社区贡献
- **翻译**: 通过Crowdin参与翻译

## 使用案例

### 解析器支持
- **worldstate-parser**: 为世界状态解析器提供数据支持
- **物品数据**: 为物品解析提供映射数据
- **本地化**: 为多语言应用提供翻译支持

### 独立使用
- **游戏数据查询**: 查询游戏节点和任务信息
- **本地化应用**: 多语言游戏应用开发
- **数据分析**: 游戏数据分析和统计

## 与其他项目的关系

### 核心依赖
- **warframe-worldstate-parser**: 主要消费者
- **warframe-items**: 物品数据补充
- **其他WFCD项目**: 数据共享和支持

### 数据流向
```
游戏官方数据 → warframe-worldstate-data → warframe-worldstate-parser → 应用程序
```

这个项目在整个WFCD生态系统中扮演着基础数据层的角色，为上层应用提供必要的数据支持和本地化服务。 
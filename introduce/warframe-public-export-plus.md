# warframe-public-export-plus 详细介绍

## 项目概述

**warframe-public-export-plus** 是最完整的 Warframe 数据源项目，它包含了官方 Public Export 中缺失的所有内容和更多增强功能。该项目将数据和本地化完全分离，为开发者提供了最全面、最准确的 Warframe 游戏数据。

## 主要特色

### 1. 完整的数据覆盖
- **超越官方导出**: 包含官方 Public Export 中缺失的所有数据
- **全面的物品信息**: 涵盖所有游戏内物品、武器、Warframe 等
- **详细的属性数据**: 包含完整的属性值和计算公式
- **关联数据**: 物品间的关联关系和依赖信息

### 2. 本地化分离设计
- **数据与翻译分离**: Export*.json 存储数据，dict.*.json 存储翻译
- **多语言支持**: 支持15种语言的完整本地化
- **无重复数据**: 避免翻译数据的重复存储
- **开发友好**: 便于开发者处理本地化需求

### 3. 图像资源管理
- **图像路径**: 提供完整的图像路径信息
- **多种访问方式**: 支持本地导出、在线查看、CDN访问
- **质量选择**: 提供不同质量级别的图像资源
- **压缩优化**: 优化的图像压缩和加载

## 数据结构

### 核心导出文件
```
warframe-public-export-plus/
├── ExportAbilities.json       # 技能数据
├── ExportAchievements.json    # 成就数据
├── ExportArcanes.json         # 秘奥数据
├── ExportAvionics.json        # 军械数据
├── ExportBoosters.json        # 增幅器数据
├── ExportBoosterPacks.json    # 增幅器包数据
├── ExportBounties.json        # 赏金任务数据
├── ExportBundles.json         # 捆绑包数据
├── ExportChallenges.json      # 挑战数据
├── ExportCodex.json           # 图鉴数据
├── ExportCustoms.json         # 自定义数据
├── ExportDojoRecipes.json     # 道场配方数据
├── ExportDrones.json          # 无人机数据
├── ExportEmailItems.json     # 邮件物品数据
├── ExportEnemies.json         # 敌人数据
├── ExportFlavour.json         # 风味文本数据
├── ExportFocusUpgrades.json   # 专精升级数据
├── ExportFusionBundles.json   # 融合包数据
├── ExportGear.json            # 装备数据
├── ExportImages.json          # 图像数据
├── ExportIntrinsics.json      # 内在能力数据
├── ExportKeys.json            # 钥匙数据
├── ExportMisc.json            # 杂项数据
├── ExportModSet.json          # 模组套装数据
├── ExportNightwave.json       # 夜灵电波数据
├── ExportRailjackWeapons.json # 帝国军舰武器数据
├── ExportRecipes.json         # 配方数据
├── ExportRegions.json         # 区域数据
├── ExportRelics.json          # 遗物数据
├── ExportResources.json       # 资源数据
├── ExportRewards.json         # 奖励数据
├── ExportSentinels.json       # 机械伙伴数据
├── ExportSyndicates.json      # 集团数据
├── ExportSystems.json         # 系统数据
├── ExportTextIcons.json       # 文本图标数据
├── ExportUpgrades.json        # 升级数据
├── ExportVendors.json         # 商人数据
├── ExportVirtuals.json        # 虚拟数据
├── ExportWarframes.json       # 战甲数据
├── ExportWeapons.json         # 武器数据
└── dict.[lang].json           # 各语言翻译文件
```

### 本地化文件
```
├── dict.en.json    # 英语翻译
├── dict.zh.json    # 中文翻译
├── dict.ja.json    # 日语翻译
├── dict.ko.json    # 韩语翻译
├── dict.de.json    # 德语翻译
├── dict.fr.json    # 法语翻译
├── dict.es.json    # 西班牙语翻译
├── dict.it.json    # 意大利语翻译
├── dict.pl.json    # 波兰语翻译
├── dict.pt.json    # 葡萄牙语翻译
├── dict.ru.json    # 俄语翻译
├── dict.tc.json    # 繁体中文翻译
├── dict.th.json    # 泰语翻译
├── dict.tr.json    # 土耳其语翻译
└── dict.uk.json    # 乌克兰语翻译
```

## 重要数据格式

### 1. 武器数据 (ExportWeapons.json)
```json
{
  "uniqueName": "/Lotus/Weapons/Tenno/Rifle/LotusRifle",
  "name": "Braton",
  "description": "A standard assault rifle used by Tenno forces.",
  "damagePerShot": [
    {"DamageType": "DT_IMPACT", "Damage": 8.8},
    {"DamageType": "DT_PUNCTURE", "Damage": 22.0},
    {"DamageType": "DT_SLASH", "Damage": 4.4}
  ],
  "fireRate": 8.8,
  "accuracy": 28.6,
  "criticalChance": 0.12,
  "criticalMultiplier": 2.0,
  "statusChance": 0.18,
  "reloadTime": 2.0,
  "magazineSize": 60,
  "maxAmmo": 540,
  "icon": "/Lotus/Interface/Icons/Weapons/LotusRifle.png",
  "behaviours": ["WEAPON_BEHAVIOURS_RIFLES"]
}
```

### 2. 战甲数据 (ExportWarframes.json)
```json
{
  "uniqueName": "/Lotus/Powersuits/Ninja/Ninja",
  "name": "Excalibur",
  "description": "A master of blade and gun, Excalibur is versatile and deadly.",
  "health": 100,
  "shield": 100,
  "armor": 225,
  "power": 100,
  "sprintSpeed": 1.0,
  "abilities": [
    "/Lotus/Powersuits/Ninja/SlashDashAbility",
    "/Lotus/Powersuits/Ninja/RadialBlindAbility",
    "/Lotus/Powersuits/Ninja/RadialJavelinAbility",
    "/Lotus/Powersuits/Ninja/ExaltedBladeAbility"
  ],
  "icon": "/Lotus/Interface/Icons/Warframes/Excalibur.png",
  "passiveDescription": "Sword weapon attacks deal 10% more damage and have 10% more Attack Speed."
}
```

### 3. 遗物数据 (ExportRelics.json)
```json
{
  "uniqueName": "/Lotus/Types/Keys/VoidKey1",
  "name": "Lith A1 Relic",
  "description": "Contains various Prime items and Blueprints.",
  "rewards": [
    {
      "itemName": "/Lotus/Weapons/Tenno/Bow/PrimeBow/PrimeBowLimb",
      "rarity": "COMMON",
      "chance": 25.33
    },
    {
      "itemName": "/Lotus/Weapons/Tenno/Shotgun/PrimeShotgun/PrimeShotgunBarrel",
      "rarity": "UNCOMMON", 
      "chance": 11.0
    }
  ],
  "icon": "/Lotus/Interface/Icons/VoidProjections/VoidProjectionBronze.png"
}
```

### 4. 本地化数据 (dict.*.json)
```json
{
  "/Lotus/Language/Items/BratonName": "Braton",
  "/Lotus/Language/Items/BratonDescription": "A standard assault rifle used by Tenno forces.",
  "/Lotus/Language/Warframes/ExcaliburName": "Excalibur",
  "/Lotus/Language/Warframes/ExcaliburDescription": "A master of blade and gun, Excalibur is versatile and deadly."
}
```

## 使用方法

### 1. 基本数据访问
```javascript
// 加载数据
const weapons = require('./ExportWeapons.json');
const warframes = require('./ExportWarframes.json');
const translations = require('./dict.en.json');

// 获取武器信息
const braton = weapons.find(w => w.uniqueName === '/Lotus/Weapons/Tenno/Rifle/LotusRifle');
console.log(braton.name, braton.damagePerShot);

// 获取本地化名称
const localizedName = translations['/Lotus/Language/Items/BratonName'];
```

### 2. 图像资源访问
```javascript
// 方法1: 本地导出 (使用 Puxtril's Warframe Exporter)
// 从游戏文件中导出纹理

// 方法2: browse.wf 在线查看
const imageUrl = `https://browse.wf${item.icon}`;

// 方法3: CDN访问 (如果有contentHash)
const cdnUrl = `https://content.warframe.com/PublicExport${item.icon}!${item.contentHash}`;

// 方法4: 论坛媒体 (对于徽章)
const forumUrl = `https://media.invisioncic.com/Mwarframe/pages_media/${item.forumName}.png`;
```

### 3. 数据查询示例
```javascript
// 查找特定类型的武器
const rifles = weapons.filter(w => 
  w.behaviours && w.behaviours.includes('WEAPON_BEHAVIOURS_RIFLES')
);

// 查找高暴击率武器
const highCritWeapons = weapons.filter(w => 
  w.criticalChance > 0.2
);

// 查找特定战甲的技能
const excalibur = warframes.find(w => w.name === 'Excalibur');
const abilities = excalibur.abilities;
```

## 技术细节

### 数据特殊说明

#### ExportDojoRecipes
- **价格调整**: `price` (积分)、`skipTimePrice` (白金) 和 `ingredients` 是月族规模的
- **规模转换**: 其他规模需要除以相应倍数，最小值为1
- **鬼魂氏族**: 除以100

#### ExportRegions
- **交火任务**: 通过 `secondaryFactionIndex` 字段检测
- **特殊节点**: Tyana Pass (SolNode450) 使用特殊的派系标签

#### ExportRecipes
- **名称构建**: 使用 `/Lotus/Language/Items/BlueprintAndItem` 模板
- **参数替换**: `|ITEM|` 替换为结果物品名称

#### ExportRelics
- **名称构建**: 使用 `category` 和 `era` 字段配合本地化字符串
- **概率系统**: 基于稀有度的概率分配

#### ExportRewards
- **特殊概率**: 某些奖励使用 `rarity` 而非 `probability`
- **遗物系统**: 概率取决于遗物精炼级别
- **商店物品**: 以 `/Lotus/StoreItems/` 开头的物品需要替换前缀

#### ExportUpgrades
- **重名处理**: 多个模组可能同名，需要检查 `isStarter` 和 `isFrivilous` 字段
- **挑战组合**: 使用特定的组合器字符串

#### ExportVendors
- **商店物品**: 只能销售 StoreItems，需要相应的转换处理
- **捆绑包**: 非 `/Lotus/StoreItems/` 开头的物品在 ExportBundles 中

#### ExportWarframes
- **等级缩放**: `health`、`shield`、`armor`、`power` 值为0级状态
- **升级公式**: 需要使用特定公式计算不同等级的属性

#### ExportWeapons
- **非武器物品**: 通过 `behaviours` 字段缺失来过滤
- **Kitgun特殊**: 具有 `primeOmegaAttenuation` 字段（倾向性）
- **伤害系统**: 使用 `damagePerShot` 数组而非 `behaviours`

## 性能优化

### 数据加载策略
```javascript
// 按需加载特定数据
const loadWeapons = () => require('./ExportWeapons.json');
const loadWarframes = () => require('./ExportWarframes.json');

// 缓存常用数据
const dataCache = new Map();
const getCachedData = (filename) => {
  if (!dataCache.has(filename)) {
    dataCache.set(filename, require(filename));
  }
  return dataCache.get(filename);
};
```

### 查询优化
```javascript
// 创建索引以加速查询
const createIndex = (items, keyField) => {
  const index = new Map();
  items.forEach(item => {
    index.set(item[keyField], item);
  });
  return index;
};

// 使用示例
const weaponsIndex = createIndex(weapons, 'uniqueName');
const weapon = weaponsIndex.get('/Lotus/Weapons/Tenno/Rifle/LotusRifle');
```

## 数据完整性验证

### 验证脚本
```javascript
// 验证数据完整性
const validateData = (data, requiredFields) => {
  return data.every(item => 
    requiredFields.every(field => item.hasOwnProperty(field))
  );
};

// 验证武器数据
const weaponRequiredFields = ['uniqueName', 'name', 'damagePerShot'];
const isWeaponDataValid = validateData(weapons, weaponRequiredFields);

// 验证本地化数据
const validateTranslations = (translations, items) => {
  const missingTranslations = [];
  items.forEach(item => {
    const translationKey = `/Lotus/Language/Items/${item.name}Name`;
    if (!translations[translationKey]) {
      missingTranslations.push(translationKey);
    }
  });
  return missingTranslations;
};
```

## 应用场景

### 1. 游戏数据库应用
```javascript
// 构建物品数据库
const buildItemDatabase = () => {
  const database = {
    weapons: loadWeapons(),
    warframes: loadWarframes(),
    mods: loadUpgrades(),
    // 其他数据...
  };
  
  // 添加搜索功能
  database.search = (query, category) => {
    return database[category].filter(item => 
      item.name.toLowerCase().includes(query.toLowerCase())
    );
  };
  
  return database;
};
```

### 2. 计算器应用
```javascript
// 武器DPS计算器
const calculateWeaponDPS = (weapon) => {
  const totalDamage = weapon.damagePerShot.reduce((sum, dmg) => sum + dmg.Damage, 0);
  const critMultiplier = 1 + (weapon.criticalChance * (weapon.criticalMultiplier - 1));
  return totalDamage * weapon.fireRate * critMultiplier;
};

// 战甲属性计算器
const calculateWarframeStats = (warframe, rank) => {
  const rankMultiplier = 1 + (rank * 0.5);
  return {
    health: warframe.health * rankMultiplier,
    shield: warframe.shield * rankMultiplier,
    armor: warframe.armor,
    power: warframe.power * rankMultiplier
  };
};
```

### 3. 本地化应用
```javascript
// 多语言支持
class LocalizationManager {
  constructor(language = 'en') {
    this.language = language;
    this.translations = require(`./dict.${language}.json`);
  }
  
  translate(key) {
    return this.translations[key] || key;
  }
  
  getLocalizedName(item) {
    const nameKey = `/Lotus/Language/Items/${item.name}Name`;
    return this.translate(nameKey);
  }
}
```

## 开发者信息

- **维护者**: Calamity Inc.
- **许可证**: 未明确指定
- **仓库**: https://github.com/calamity-inc/warframe-public-export-plus
- **在线浏览**: https://browse.wf/
- **数据格式**: JSON

## 相关工具

### 浏览工具
- **browse.wf**: 在线浏览和搜索数据
- **Puxtril's Warframe Exporter**: 本地图像导出工具

### 开发工具
- **类型验证**: validate-typings.pluto
- **键值验证**: validate-keys.pluto
- **补充数据**: supplementals/ 目录

## 数据更新

### 更新机制
- **跟踪游戏更新**: 监控游戏版本变化
- **自动数据提取**: 从游戏文件自动提取数据
- **质量验证**: 确保数据完整性和准确性
- **版本控制**: 维护数据版本历史

### 社区贡献
- **数据校验**: 社区帮助验证数据准确性
- **错误报告**: 通过 GitHub Issues 报告问题
- **功能建议**: 建议新的数据类型或功能

这个项目为整个 Warframe 开发者社区提供了最全面、最准确的数据基础，是构建 Warframe 相关应用的理想选择。 
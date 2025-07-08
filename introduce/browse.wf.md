# browse.wf 详细介绍

## 项目概述

**browse.wf** 是一个基于Web的 Warframe 数据浏览和查询工具。它使用 warframe-public-export-plus 的数据，提供了一个用户友好的界面来浏览、搜索和查看 Warframe 的所有游戏内容。该项目是开源的，为社区提供了一个强大的数据查询平台。

## 主要功能

### 1. 数据浏览与搜索
- **全文搜索**: 在所有游戏数据中进行快速搜索
- **分类浏览**: 按物品类型、武器类别等分类浏览
- **实时过滤**: 动态过滤和排序功能
- **高级查询**: 支持复杂的查询条件

### 2. 详细信息展示
- **物品详情**: 显示物品的完整属性和统计数据
- **关联信息**: 显示相关物品和依赖关系
- **图像展示**: 高质量的游戏内图像显示
- **历史数据**: 物品的变更历史和补丁信息

### 3. 专用工具
- **颜色选择器**: 游戏内颜色配置工具
- **雕像计算器**: Ayatan 雕像价值计算
- **裂缝追踪**: 虚空裂缝信息追踪
- **商人库存**: 虚空商人巴洛基尔的库存查询

## 技术架构

### 核心文件结构
```
browse.wf/
├── index.php              # 主页面
├── live.php              # 实时数据页面
├── profile.php           # 配置文件页面
├── rivencalc.php         # Riven计算器
├── kimulacrum.php        # 模拟室工具
├── color-picker.php      # 颜色选择器
├── invigorations.php     # 激励系统
├── arbys.php             # 仲裁追踪
├── prime-vault.php       # Prime保险库
├── inventory.php         # 库存管理
├── glyphs.php            # 徽章查看器
├── text-icons.php        # 文本图标
├── common.js             # 通用JavaScript
├── components/           # 组件目录
├── supplemental-data/    # 补充数据
├── favicon.ico           # 网站图标
└── 404.php              # 404错误页面
```

### 技术栈
- **后端**: PHP
- **前端**: HTML5, CSS3, JavaScript
- **数据源**: warframe-public-export-plus
- **图像服务**: 内置图像托管和CDN
- **实时数据**: 世界状态API集成

## 核心功能详解

### 1. 主页面 (index.php)
```php
// 主要功能
- 数据搜索和浏览
- 物品分类展示
- 高级筛选选项
- 响应式设计界面

// 核心特性
- 自动完成搜索
- 实时结果更新
- 多维度筛选
- 书签和分享功能
```

### 2. 实时数据 (live.php)
```php
// 世界状态信息
- 警报任务
- 入侵事件
- 虚空裂缝
- 突击任务
- 夜灵电波
- 周期信息
- 虚空商人状态

// 更新机制
- 实时数据获取
- 自动刷新
- 状态变化通知
- 历史数据记录
```

### 3. Riven计算器 (rivencalc.php)
```php
// 计算功能
- Riven模组属性计算
- 伤害输出评估
- 最优配置建议
- 对比分析工具

// 高级功能
- 多武器对比
- 构筑方案推荐
- 市场价值评估
- 统计数据分析
```

### 4. 颜色选择器 (color-picker.php)
```php
// 颜色工具
- 游戏内颜色预览
- 色板管理
- 颜色代码转换
- 配色方案保存

// 特殊功能
- 社区颜色分享
- 流行配色推荐
- 色彩搭配建议
- 导出功能
```

### 5. 模拟室工具 (kimulacrum.php)
```php
// 测试环境
- 伤害计算模拟
- 武器性能测试
- 敌人类型选择
- 环境变量设置

// 分析功能
- DPS计算
- 效率分析
- 对比测试
- 结果导出
```

## 数据处理与管理

### 数据集成
```php
// warframe-public-export-plus 集成
class DataManager {
    public function loadWeapons() {
        return json_decode(file_get_contents('ExportWeapons.json'), true);
    }
    
    public function loadWarframes() {
        return json_decode(file_get_contents('ExportWarframes.json'), true);
    }
    
    public function getLocalizedName($item, $language = 'en') {
        $dict = json_decode(file_get_contents("dict.{$language}.json"), true);
        return $dict[$item['nameKey']] ?? $item['name'];
    }
}
```

### 缓存系统
```php
// 数据缓存
class CacheManager {
    private $cacheDir = 'cache/';
    
    public function get($key) {
        $file = $this->cacheDir . md5($key) . '.cache';
        if (file_exists($file) && (time() - filemtime($file)) < 3600) {
            return unserialize(file_get_contents($file));
        }
        return false;
    }
    
    public function set($key, $data) {
        $file = $this->cacheDir . md5($key) . '.cache';
        file_put_contents($file, serialize($data));
    }
}
```

### 搜索引擎
```php
// 搜索功能
class SearchEngine {
    public function search($query, $category = null) {
        $results = [];
        $data = $this->loadAllData();
        
        foreach ($data as $item) {
            if ($this->matchesQuery($item, $query)) {
                if (!$category || $item['category'] === $category) {
                    $results[] = $item;
                }
            }
        }
        
        return $this->rankResults($results, $query);
    }
    
    private function matchesQuery($item, $query) {
        return stripos($item['name'], $query) !== false ||
               stripos($item['description'], $query) !== false;
    }
}
```

## 用户界面设计

### 响应式布局
```css
/* 主要布局 */
.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

.grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
}

/* 移动端适配 */
@media (max-width: 768px) {
    .grid {
        grid-template-columns: 1fr;
    }
}
```

### 交互功能
```javascript
// 搜索功能
class SearchUI {
    constructor() {
        this.searchInput = document.getElementById('search');
        this.resultsContainer = document.getElementById('results');
        this.initializeEvents();
    }
    
    initializeEvents() {
        this.searchInput.addEventListener('input', (e) => {
            this.performSearch(e.target.value);
        });
    }
    
    async performSearch(query) {
        if (query.length < 2) return;
        
        const response = await fetch(`/api/search?q=${encodeURIComponent(query)}`);
        const results = await response.json();
        this.displayResults(results);
    }
    
    displayResults(results) {
        this.resultsContainer.innerHTML = results.map(item => 
            this.createItemCard(item)
        ).join('');
    }
}
```

## API 接口

### 数据 API
```php
// RESTful API 端点
// GET /api/weapons - 获取所有武器
// GET /api/weapons/{id} - 获取特定武器
// GET /api/warframes - 获取所有战甲
// GET /api/search?q={query} - 搜索功能
// GET /api/worldstate - 获取世界状态

class APIController {
    public function getWeapons() {
        header('Content-Type: application/json');
        $weapons = $this->dataManager->loadWeapons();
        echo json_encode($weapons);
    }
    
    public function getWeapon($id) {
        header('Content-Type: application/json');
        $weapon = $this->dataManager->getWeaponById($id);
        echo json_encode($weapon);
    }
    
    public function search($query) {
        header('Content-Type: application/json');
        $results = $this->searchEngine->search($query);
        echo json_encode($results);
    }
}
```

### 图像服务
```php
// 图像代理服务
class ImageProxy {
    public function getImage($path) {
        // 检查本地缓存
        $localPath = $this->getLocalImagePath($path);
        if (file_exists($localPath)) {
            $this->serveImage($localPath);
            return;
        }
        
        // 从游戏资源获取
        $imageData = $this->fetchGameImage($path);
        if ($imageData) {
            $this->cacheImage($localPath, $imageData);
            $this->serveImageData($imageData);
        } else {
            $this->serve404();
        }
    }
    
    private function serveImage($path) {
        $mimeType = $this->getMimeType($path);
        header("Content-Type: {$mimeType}");
        readfile($path);
    }
}
```

## 专用工具详解

### 1. 仲裁追踪器 (arbys.php)
```php
// 仲裁数据管理
class ArbitrationTracker {
    public function getCurrentArbitration() {
        $worldState = $this->getWorldState();
        return $worldState['arbitrations'] ?? null;
    }
    
    public function getArbitrationHistory() {
        return json_decode(file_get_contents('arbys.txt'), true);
    }
    
    public function predictNextArbitration() {
        // 基于历史数据预测
        $history = $this->getArbitrationHistory();
        return $this->calculateNextRotation($history);
    }
}
```

### 2. Prime保险库追踪 (prime-vault.php)
```php
// Prime保险库管理
class PrimeVault {
    public function getCurrentVault() {
        return $this->parseVaultData();
    }
    
    public function getVaultHistory() {
        return $this->loadVaultHistory();
    }
    
    public function getUnvaultedItems() {
        $vault = $this->getCurrentVault();
        return $vault['unvaulted'] ?? [];
    }
}
```

### 3. 激励系统 (invigorations.php)
```php
// Helminth激励追踪
class InvigorationTracker {
    public function getCurrentInvigorations() {
        $worldState = $this->getWorldState();
        return $worldState['helminthInvigorations'] ?? [];
    }
    
    public function getInvigorationDetails($warframeName) {
        $invigorations = $this->getCurrentInvigorations();
        return $this->findInvigorationForWarframe($invigorations, $warframeName);
    }
}
```

## 性能优化

### 缓存策略
```php
// 多级缓存系统
class CacheStrategy {
    // 内存缓存
    private static $memoryCache = [];
    
    // 文件缓存
    private $fileCache;
    
    // Redis缓存（如果可用）
    private $redisCache;
    
    public function get($key) {
        // 1. 检查内存缓存
        if (isset(self::$memoryCache[$key])) {
            return self::$memoryCache[$key];
        }
        
        // 2. 检查Redis缓存
        if ($this->redisCache && $data = $this->redisCache->get($key)) {
            self::$memoryCache[$key] = $data;
            return $data;
        }
        
        // 3. 检查文件缓存
        if ($data = $this->fileCache->get($key)) {
            self::$memoryCache[$key] = $data;
            if ($this->redisCache) {
                $this->redisCache->set($key, $data, 3600);
            }
            return $data;
        }
        
        return null;
    }
}
```

### 前端优化
```javascript
// 延迟加载
class LazyLoader {
    constructor() {
        this.observer = new IntersectionObserver(this.handleIntersection.bind(this));
    }
    
    observe(element) {
        this.observer.observe(element);
    }
    
    handleIntersection(entries) {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                this.loadContent(entry.target);
                this.observer.unobserve(entry.target);
            }
        });
    }
    
    loadContent(element) {
        const src = element.dataset.src;
        if (src) {
            element.src = src;
        }
    }
}

// 图像优化
class ImageOptimizer {
    static async loadOptimizedImage(src, quality = 'medium') {
        const sizes = {
            low: '?w=200&q=60',
            medium: '?w=400&q=80',
            high: '?w=800&q=95'
        };
        
        return `${src}${sizes[quality]}`;
    }
}
```

## 部署与维护

### 服务器配置
```nginx
# Nginx 配置
server {
    listen 80;
    server_name browse.wf;
    root /var/www/browse.wf;
    
    # PHP配置
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }
    
    # 静态资源缓存
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # API路由
    location /api/ {
        try_files $uri $uri/ /api/index.php?$query_string;
    }
}
```

### 监控与日志
```php
// 错误监控
class ErrorMonitor {
    public function logError($error, $context = []) {
        $logEntry = [
            'timestamp' => date('Y-m-d H:i:s'),
            'error' => $error,
            'context' => $context,
            'user_agent' => $_SERVER['HTTP_USER_AGENT'] ?? '',
            'ip' => $_SERVER['REMOTE_ADDR'] ?? ''
        ];
        
        file_put_contents('logs/error.log', json_encode($logEntry) . "\n", FILE_APPEND);
    }
    
    public function logAccess() {
        $logEntry = [
            'timestamp' => date('Y-m-d H:i:s'),
            'method' => $_SERVER['REQUEST_METHOD'],
            'uri' => $_SERVER['REQUEST_URI'],
            'user_agent' => $_SERVER['HTTP_USER_AGENT'] ?? '',
            'ip' => $_SERVER['REMOTE_ADDR'] ?? ''
        ];
        
        file_put_contents('logs/access.log', json_encode($logEntry) . "\n", FILE_APPEND);
    }
}
```

## 开发者信息

- **维护者**: Calamity Inc.
- **许可证**: MIT
- **仓库**: https://github.com/calamity-inc/browse.wf
- **网站**: https://browse.wf/
- **技术栈**: PHP, JavaScript, HTML5, CSS3

## 社区支持

### 贡献方式
- **功能请求**: 通过 GitHub Issues 提交功能请求
- **错误报告**: 报告网站问题和数据错误
- **代码贡献**: 提交Pull Request改进代码
- **数据验证**: 帮助验证和改进数据准确性

### 用户反馈
- **用户体验**: 收集用户使用反馈
- **功能建议**: 新功能和改进建议
- **性能问题**: 性能问题报告和优化建议
- **兼容性**: 不同浏览器和设备的兼容性问题

## 未来发展

### 功能扩展
- **移动应用**: 开发原生移动应用
- **离线功能**: 支持离线数据浏览
- **用户账户**: 个性化设置和收藏功能
- **社区功能**: 用户评论和评分系统

### 技术改进
- **性能优化**: 进一步提升加载速度
- **搜索改进**: 更智能的搜索算法
- **数据同步**: 实时数据同步机制
- **API扩展**: 更丰富的API接口

browse.wf 为 Warframe 社区提供了一个强大、易用的数据查询平台，是开发者和玩家获取游戏信息的重要工具。它的开源性质也使得社区能够持续改进和扩展其功能。 
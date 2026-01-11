# 六边形消除 (SixClear)
玩法有改动的六边形消除游戏

## 项目概述 (Project Overview)
- **类型**: Cocos Creator 2.4.15 JavaScript 游戏
- **游戏**: 六边形消除益智游戏
- **引擎**: cocos2d-html5
- **语言**: JavaScript (ES6 target)
- **平台支持**: Web-mobile, 微信小游戏, Android, iOS

## 兼容性 (Compatibility)
cocos creator 2.0+

## 在线试玩 (Online Demo)
[体验地址](https://zx6733090.github.io/)

## 开发环境设置 (Development Setup)

### 系统要求 (Requirements)
- Cocos Creator 2.0+ 编辑器
- Node.js (可选，用于开发工具)
- 支持的构建平台：Web, 微信小游戏, Android, iOS

### 构建命令 (Build Commands)
由于这是 Cocos Creator 项目，构建通过 Cocos Creator 编辑器完成：

```bash
# 通过 Cocos Creator GUI 为不同平台构建
# - Web Mobile: 为 HTML5 网页浏览器构建
# - 微信小游戏: 为微信小游戏平台构建
# - Android: 为 Android 设备构建
# - iOS: 为 iOS 设备构建
```

### 开发工作流 (Development Workflow)
1. 在 Cocos Creator 2.0+ 中打开项目
2. 使用 Cocos Creator 内置预览进行测试
3. 通过 Cocos Creator 的构建面板为目标平台构建
4. 未检测到命令行构建系统

### 测试 (Testing)
- **手动测试**: 使用 Cocos Creator 预览和设备模拟器
- **平台测试**: 在目标平台上构建后测试
- **无自动化测试**: 项目中未检测到测试框架

## 项目结构 (Project Structure)
```
assets/
├── Script/          # 游戏逻辑脚本
├── Texture/         # 游戏资源 (图片, 精灵)
└── migration/       # 迁移脚本
SubGame/
└── assets/js/       # 子游戏脚本
settings/            # Cocos Creator 项目设置
```

## 代码规范 (Code Style Guidelines)

### 导入/导出模式
```javascript
// 使用 CommonJS require() 进行模块导入
var config = require('config');
var tooltip = require('tooltip');
var gameScene = require('game_scene');

// 使用 module.exports 导出
module.exports = config;
```

### 类定义模式
```javascript
// 标准 Cocos Creator 组件模式
cc.Class({
    extends: cc.Component,
    
    properties: {
        // 组件属性
        chess: {
            default: [],
            type: cc.Prefab
        },
        gameScene: gameScene,
        fortuneLabel: cc.Label
    },
    
    // 生命周期回调:
    start() {
        // 初始化代码
    },
    
    update(dt) {
        // 每帧更新逻辑
    }
});
```

### 命名约定
- **变量和函数**: camelCase
- **类/组件**: PascalCase  
- **常量**: UPPER_SNAKE_CASE
- **文件名**: lowercase_with_underscores

### 代码格式化
- **缩进**: 4个空格 (源文件中观察到)
- **行长度**: 尽量保持在120字符以内
- **间距**: 函数调用和条件语句使用适当空格

## 开发最佳实践 (Development Best Practices)

1. **遵循 Cocos Creator 生命周期**: 适当使用 `onLoad()`, `start()`, `update()`
2. **组件通信**: 使用 `require()` 进行模块依赖
3. **事件驱动架构**: 使用 Cocos Creator 事件系统
4. **资源管理**: 高效使用预制件和精灵帧
5. **平台抽象**: 使用前检查平台特定功能
6. **内存管理**: 正确清理对象和移除监听器
7. **代码组织**: 将相关功能保持在单独模块中

## 常见陷阱 (Common Pitfalls to Avoid)

1. **不要使用 ES6 模块** - 坚持使用 CommonJS require()
2. **不要忘记清理** 事件监听器和计划任务
3. **不要混用中英文注释** - 保持一致性
4. **不要忽略平台兼容性** - 使用前检查功能
5. **不要创建内存泄漏** - 正确移除节点和组件

## 测试指南 (Testing Guidelines)

1. **手动测试**: 在 Cocos Creator 预览中彻底测试游戏机制
2. **平台测试**: 在目标平台上测试构建 (Web, 微信, 移动端)
3. **性能测试**: 监控帧率和内存使用
4. **兼容性测试**: 在不同设备和屏幕尺寸上测试

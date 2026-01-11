# AGENTS.md - Development Guidelines for SixClear Game

## Project Overview
- **Type**: Cocos Creator 2.4.15 JavaScript Game
- **Game**: Hexagonal puzzle elimination game (六边形消除)
- **Engine**: cocos2d-html5
- **Language**: JavaScript (ES6 target)
- **Platform Support**: Web-mobile, WeChat Mini Game, Android, iOS

## Build & Development Commands

### Cocos Creator Build Commands
Since this is a Cocos Creator project, building is done through the Cocos Creator editor:

```bash
# Build for different platforms (via Cocos Creator GUI)
# - Web Mobile: Build for HTML5 web browsers
# - WeChat Mini Game: Build for 微信小游戏
# - Android: Build for Android devices
# - iOS: Build for iOS devices
```

### Development Workflow
1. Open project in Cocos Creator 2.0+
2. Use Cocos Creator's built-in preview for testing
3. Build via Cocos Creator's Build panel for target platforms
4. No command-line build system detected

### Testing
- **Manual Testing**: Use Cocos Creator's preview and device simulators
- **Platform Testing**: Test on target platforms after build
- **No Automated Tests**: No test framework detected in project

## Code Style Guidelines

### File Structure & Organization
```
assets/
├── Script/          # Game logic scripts
├── Texture/         # Game assets (images, sprites)
└── migration/       # Migration scripts
SubGame/
└── assets/js/       # Sub-game scripts
settings/            # Cocos Creator project settings
```

### Import/Export Patterns
```javascript
// Use CommonJS require() for module imports
var config = require('config');
var tooltip = require('tooltip');
var gameScene = require('game_scene');

// Export using module.exports
module.exports = config;
```

### Class Definition Pattern
```javascript
// Standard Cocos Creator component pattern
cc.Class({
    extends: cc.Component,
    
    properties: {
        // Component properties here
        chess: {
            default: [],
            type: cc.Prefab
        },
        gameScene: gameScene,
        fortuneLabel: cc.Label
    },
    
    // LIFE-CYCLE CALLBACKS:
    // onLoad () {},
    start() {
        // Initialization code
    },
    
    update(dt) {
        // Per-frame update logic
    }
});
```

### Naming Conventions

#### Variables & Functions
- **camelCase** for variables and functions
- **PascalCase** for classes/components
- **UPPER_SNAKE_CASE** for constants

```javascript
var chessManager = require('chess');
var configData = {};
var MAX_SCORE = 1000;

function handleTouchStart() {
    // Function implementation
}

function updateToolState() {
    // Function implementation
}
```

#### File Names
- **lowercase_with_underscores** for script files
- **PascalCase** for component class names (inferred from usage)

```javascript
// File: game_scene.js
// File: chess.js
// File: home.js
// File: config.js
```

### Code Formatting

#### Indentation
- **4 spaces** for indentation (observed in source files)
- **No tabs** - use spaces consistently

#### Line Length
- Keep lines under **120 characters** when possible
- Break long function calls across multiple lines

#### Spacing
```javascript
// Function calls with proper spacing
this.node.getChildByName('tool' + i).getComponent(cc.Toggle).interactable = i < idx;

// Array/object spacing
var config = {
    game: {},
    version: "1.0.0"
};

// Conditional spacing
if (config.toolNum[type] > 0) {
    // Implementation
}
```

### Type Usage & Patterns

#### Cocos Creator Types
```javascript
// Common Cocos Creator types used
cc.Component          // Base component class
cc.Node              // Scene node
cc.Prefab            // Prefab assets
cc.Label             // UI label component
cc.Toggle            // UI toggle component
cc.Animation         // Animation component
cc.SpriteFrame       // Sprite frame assets
cc.Vec2              // 2D vector
cc.Color             // Color values
```

#### Property Definitions
```javascript
properties: {
    // Array of prefabs
    chess: {
        default: [],
        type: cc.Prefab
    },
    
    // Single component reference
    fortuneLabel: cc.Label,
    
    // Custom script component
    tooltip: tooltip,
    
    // Required script with type
    gameScene: gameScene
}
```

### Error Handling Patterns

#### Null/Undefined Checks
```javascript
// Common pattern for null checks
if (!ap) return false;

// Safe property access
if (this.cells[key]) {
    this.cells[key].getComponent('cell').died();
}
```

#### Try-Catch Usage
```javascript
// Limited try-catch usage observed
// When used, typically for asset loading
cc.loader.loadRes('config', (err, data) => {
    console.log("load config err:", err);
    if (!err || err.status == 0 || err.status == 200) {
        config.game = JSON.parse(data);
    }
});
```

### Comment Style

#### File Headers
```javascript
// Learn cc.Class:
//  - [Chinese] http://docs.cocos.com/creator/manual/zh/scripting/class.html
//  - [English] http://www.cocos2d-x.org/docs/creator/en/scripting/class.html
// Learn Attribute:
//  - [Chinese] http://docs.cocos.com/creator/manual/zh/scripting/reference/attributes.html
//  - [English] http://www.cocos2d-x.org/docs/creator/en/scripting/reference/attributes.html
// Learn life-cycle callbacks:
//  - [Chinese] http://docs.cocos.com/creator/manual/zh/scripting/life-cycle-callbacks.html
//  - [English] http://www.cocos2d-x.org/docs/creator/en/scripting/life-cycle-callbacks.html
```

#### Function Comments
```javascript
//对齐坐标到单元格中心,返回对齐后的坐标，如果 无法对齐返回null
alignPosition(pos) {
    // Implementation
}
```

### Event Handling Patterns

#### Touch Events
```javascript
// Touch event registration
this.node.on(cc.Node.EventType.TOUCH_START, (ev) => {
    // Handle touch start
});

this.node.on(cc.Node.EventType.TOUCH_MOVE, ev => {
    // Handle touch move
}, this);

this.node.on(cc.Node.EventType.TOUCH_END, endFunc);
this.node.on(cc.Node.EventType.TOUCH_CANCEL, endFunc);
```

#### Toggle Events
```javascript
// Toggle state change
toggleState(ev, type) {
    var pretp = ev.isChecked ? type : 0;
    // Handle toggle change
}
```

### Animation & Audio Patterns

#### Animation Usage
```javascript
// Play animation
this.node.getComponent(cc.Animation).play();

// Animation sequences
ins.runAction(cc.sequence(
    cc.spawn(cc.scaleTo(.5, .3), cc.moveTo(.5, cc.v2(-425, 473))),
    cc.removeSelf(true)
));
```

#### Audio Usage
```javascript
// Play sound effects
cc.audioEngine.playEffect(this.gameScene.effects[0]);

// Play background music
cc.audioEngine.playMusic(this.gameScene.effects[9], true);

// Control volume
cc.audioEngine.setEffectsVolume(v);
cc.audioEngine.setMusicVolume(v);
```

### Data Management Patterns

#### Local Storage
```javascript
// Configuration with localStorage getters/setters
var config = {
    get signTime() {
        if(!this._signTime){
            this._signTime=JSON.parse(localStorage.getItem("signTime")||"[]");
        }
        return this._signTime;
    },
    set signTime(value) {
        this._signTime=value;
        localStorage.setItem("signTime",JSON.stringify(value));
    }
};
```

#### State Management
- Use global config object for game state
- Component-level state for UI elements
- Local storage for persistent data

### Platform-Specific Code

#### WeChat Mini Game
```javascript
// WeChat platform detection
if (window.wx) {
    wx.postMessage({ type: 'set_key', key: "score", value: config.fortune });
    wx.onShow(this.handleOptions);
}
```

### Performance Considerations

#### Object Pooling
- Use `cc.instantiate()` for creating objects from prefabs
- Use `cc.removeSelf(true)` for cleanup
- Reuse objects where possible

#### Scheduling
```javascript
// Use schedule for delayed actions
this.scheduleOnce(() => {
    this.manager.clearAll();
}, .01);

// Use schedule for repeated actions
this.schedule(this.updateSubGame, .2, 20);
```

## Development Best Practices

1. **Follow Cocos Creator Lifecycle**: Use `onLoad()`, `start()`, `update()` appropriately
2. **Component Communication**: Use `require()` for module dependencies
3. **Event-Driven Architecture**: Use Cocos Creator's event system
4. **Asset Management**: Use prefabs and sprite frames efficiently
5. **Platform Abstraction**: Check for platform-specific features before use
6. **Memory Management**: Properly clean up objects and remove listeners
7. **Code Organization**: Keep related functionality in separate modules

## Common Pitfalls to Avoid

1. **Don't use ES6 modules** - stick to CommonJS require()
2. **Don't forget to clean up** event listeners and scheduled tasks
3. **Don't mix Chinese and English comments** inconsistently
4. **Don't ignore platform compatibility** - check for features before use
5. **Don't create memory leaks** - properly remove nodes and components

## Testing Guidelines

1. **Manual Testing**: Test game mechanics thoroughly in Cocos Creator preview
2. **Platform Testing**: Test builds on target platforms (Web, WeChat, Mobile)
3. **Performance Testing**: Monitor frame rate and memory usage
4. **Compatibility Testing**: Test on different devices and screen sizes

## File Extensions & Types

- `.js` - JavaScript game scripts
- `.json` - Configuration and project files
- `.meta` - Cocos Creator asset metadata
- `.png` - Texture and sprite assets
- `.d.ts` - TypeScript definition files (for Cocos Creator API)
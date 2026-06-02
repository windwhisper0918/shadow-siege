# 暗影围城 (Shadow Siege) - 开发指南

## 快速开始

### 环境要求
- Godot 4.2+ (https://godotengine.org/download)
- 推荐用 Godot Editor 编辑场景(.tscn)文件
- GDScript 无需额外配置

### 项目结构
```
shadow_siege/
├── project.godot          # 项目配置
├── scenes/                # 场景文件(需在Editor中创建)
│   ├── main_menu.tscn     # 主菜单
│   ├── game.tscn          # 游戏主场景
│   ├── enemies/           # 怪物场景
│   └── towers/            # 防御塔场景
├── scripts/               # GDScript脚本(已完成)
│   ├── game_manager.gd    # 全局游戏状态
│   ├── game_scene.gd      # 游戏主场景逻辑
│   ├── wave_manager.gd    # 波次管理
│   ├── enemy_base.gd      # 怪物基类
│   ├── tower_base.gd      # 防御塔基类
│   ├── hero_base.gd       # 英雄基类
│   ├── talent_tree.gd     # 天赋系统
│   ├── map_grid.gd        # 地图网格
│   ├── hud.gd             # HUD界面
│   └── main_menu.gd       # 主菜单
├── resources/
│   └── wave_data/
│       └── waves_forest.json  # 第一关波次数据
└── assets/                # 美术资源(待添加)
    ├── sprites/
    ├── tiles/
    └── audio/
```

### 开发步骤

1. 安装 Godot 4.2+
2. 打开项目: godot4 --path shadow_siege/
3. 在Editor中创建场景文件(.tscn)并挂载脚本
4. 添加美术资源到assets/
5. F5运行测试

### 核心脚本说明

| 脚本 | 职责 |
|------|------|
| game_manager.gd | 全局单例，管理金币/生命/天赋/经验 |
| wave_manager.gd | 波次生成控制，怪物类型和数量管理 |
| enemy_base.gd | 怪物基类，移动/受伤/死亡逻辑 |
| tower_base.gd | 防御塔基类，索敌/攻击/升级逻辑 |
| hero_base.gd | 英雄基类，技能/装备/复活逻辑 |
| map_grid.gd | 地图网格，路径/可建造区域管理 |
| talent_tree.gd | 天赋系统，3条路线9个天赋 |
| hud.gd | 顶部状态栏，金币/生命/波次显示 |

### 设计数据 (JSON配置)

所有数值平衡在 resources/wave_data/waves_forest.json 中配置：
- 怪物属性: 血量/护甲/魔抗/速度/奖励
- 塔属性: 费用/伤害/攻速/范围/类型
- 波次配置: 每波怪物组合

### 伤害公式
```
物理伤害 = 基础伤害 * 100/(100+护甲)
魔法伤害 = 基础伤害 * 100/(100+魔抗)
暴击伤害 = 最终伤害 * 2.0 (暴击率由天赋提供)
减速效果 = 基础减速 * (1 + 天赋加成)
```

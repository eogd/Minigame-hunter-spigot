# MyHunterPlugin - 猎人游戏插件

定制插件开源！这是一款为 Spigot 1.21 服务器设计的沉浸式猎人与逃亡者对抗游戏插件。玩家将被分为猎人和逃亡者两个阵营，在充满挑战和任务的环境中展开紧张刺激的追逐。

## 功能特性

*   **动态游戏流程：** 包含等待、准备、追捕和结束四个清晰的游戏阶段。
*   **可配置的出生点与时间：** 自定义猎人、逃亡者、游戏结束点及旁观者出生点，以及游戏各阶段时长。
*   **多样化的逃亡者任务：**
    *   **NPC交互任务：** 逃亡者通过与特定NPC交互并输入密码（可选）来完成任务。
    *   **旗帜收集与放置任务：** 团队合作收集并放置旗帜以达成目标。
*   **完善的积分系统：**
    *   游戏进行时，活跃玩家每秒自动获得积分。
    *   完成任务、抓捕敌人可获得额外积分。
    *   游戏结束时播报积分排名。
*   **详尽的侧边栏公告榜：** 实时显示游戏关键信息，玩家可自行开关。
*   **猎人专属特性：** 开局获得特殊武器，游戏期间无法丢弃。
*   **便捷的管理员指令：** 轻松管理游戏设置、开始游戏、设置猎人等。
*   **Citizens 插件集成：** 依赖 Citizens 插件实现任务NPC功能。

## 安装

1.  **前置要求：**
    *   确保您的服务器正在运行 Spigot 1.21 或兼容版本。
    *   **必须** 安装 [Citizens](https://www.spigotmc.org/resources/citizens.13811/) 插件，并确保其版本与您的服务器版本兼容。
2.  **步骤：**
    *   下载最新的 `MyHunterPlugin.jar` 文件。
    *   将 `MyHunterPlugin.jar` 文件放入您服务器的 `plugins` 文件夹中。
    *   重启或重新加载您的服务器。
    *   插件首次加载会自动生成 `config.yml` 配置文件。

## 配置 (`config.yml` 详解)

插件的详细行为可以通过编辑 `plugins/MyHunterPlugin/config.yml` 文件进行配置。修改后需要重启服务器或使用相应重载命令（如果插件支持）才能生效。

```yaml
# MyHunterPlugin 配置文件

#-------------------------------------------------------------------------------
# Spawn Points - 游戏出生点及传送点设置
# 格式: "世界名称,x坐标,y坐标,z坐标,水平视角(yaw),垂直视角(pitch)"
# yaw 和 pitch 是可选的, 默认为 0.
#-------------------------------------------------------------------------------
spawn_points:
  hunters: "world,0,65,10,0,0"    # 猎人出生点
  runners: "world,0,65,-10,180,0" # 逃亡者出生点
  end_game_teleport_location: "world,0,70,0,0,0" # 游戏结束后统一传送点
  spectator: "world,0,100,0,0,0" # 旁观者出生点/逃亡者被抓捕后传送点

#-------------------------------------------------------------------------------
# Time Settings - 时间设置 (所有时间单位均为: 秒)
#-------------------------------------------------------------------------------
time_settings:
  preparation: 30       # 准备阶段时长
  game_duration: 600    # 游戏主要阶段时长 (例如 600 秒 = 10 分钟)

#-------------------------------------------------------------------------------
# NPC Interaction Task - NPC交互任务设置
#-------------------------------------------------------------------------------
npc_interaction_task:
  npc_display_name: "&b任务使者" # 任务NPC在游戏内头顶显示的名称. 支持颜色代码.
  password_enabled: true         # 是否启用密码验证 (true: 启用, false: 禁用).
  password: "芝麻开门"             # NPC交互时需要输入的密码 (仅当 password_enabled 为 true 时生效).
  password_prompt_message: "&e请输入NPC的交互密码: " # NPC提示玩家输入密码时的消息.
  password_correct_message: "&a密码正确！正在处理你的任务..." # 密码正确时的提示.
  password_incorrect_message: "&c密码错误！交互失败。" # 密码错误时的提示.
  max_completions: 10           # 此任务最大可完成次数 (所有逃亡者共享).
  identity_card:                # 逃亡者初始获得的身份卡物品设置.
    material: "PAPER"           # 身份卡物品的材质 (Spigot Material 枚举名).
    display_name: "&b[身份证明]" # 身份卡物品显示的名称.
    lore:                       # 身份卡物品的Lore描述.
      - "&7这是你作为逃亡者的证明。"
      - "&e右键任务NPC并输入正确密码提交。"
  task_completion_message: "&a任务完成！你消耗了一张身份证明。" # 成功完成NPC任务时的提示.
  task_slots_full_message: "&eNPC任务名额已满，你无法再提交了！" # 任务名额已满时的提示.
  glowing_duration_seconds: 60  # 未完成NPC任务的逃亡者, 在游戏结束或名额满时, 获得发光效果的持续时间 (秒).

#-------------------------------------------------------------------------------
# Flag Task - 旗帜收集与放置任务设置
#-------------------------------------------------------------------------------
flag_task:
  enabled: true                 # 是否启用旗帜任务 (true: 启用, false: 禁用).
  required_flags_to_place: 3    # 逃亡者团队需要成功放置的旗帜总数.
  target_placement_locations:   # 旗帜目标放置点坐标列表 (精确到方块).
    - "world,10,64,10,0,0"
    - "world,-10,64,-10,0,0"
    - "world,10,64,-10,0,0"
  banner_material_to_pickup: "WHITE_BANNER" # 逃亡者需要破坏并拾取的旗帜方块的材质类型.
  flag_pickup_message: "&e你拾取了一面任务旗帜！快去指定地点放置吧！" # 拾取旗帜时的提示.
  flag_placed_message: "&a旗帜放置成功！({COUNT}/{TOTAL})" # 成功放置旗帜时的提示 ({COUNT} 当前数, {TOTAL} 总数).
  flag_already_placed_message: "&c这个目标点已经有旗帜了！换个地方试试。" # 目标点已有旗帜时的提示.
  flag_not_target_location_message: "&c这里不是旗帜的目标放置点！请对准目标点右键。" # 非目标点放置时的提示.
  task_success_broadcast: "&6[团队任务] &a逃亡者们成功收集并放置了所有旗帜！" # 旗帜任务完成时的全服广播.
  task_fail_broadcast: "&6[团队任务] &c遗憾！逃亡者们未能及时完成旗帜任务！" # 旗帜任务失败时的全服广播.
  banner_pickup_sound: "ENTITY_ITEM_PICKUP,1.0,1.0"   # 拾取旗帜音效 (SoundName,Volume,Pitch).
  banner_place_sound: "BLOCK_NOTE_BLOCK_PLING,1.0,1.2" # 成功放置旗帜音效.
  banner_place_fail_sound: "BLOCK_ANVIL_LAND,0.5,1.0"   # 放置旗帜失败音效.

#-------------------------------------------------------------------------------
# Points System - 玩家积分系统
#-------------------------------------------------------------------------------
points_system:
  enabled: true # 是否启用自定义的头顶/Tab列表积分显示.
  passive_points_per_second: 1      # 游戏进行阶段, 活跃玩家每秒获得的被动积分.
  points_per_npc_task_completion: 10 # 完成NPC交互任务获得的额外积分.
  points_per_flag_placed: 25         # 成功放置一面旗帜获得的额外积分.
  points_per_runner_caught: 50       # 猎人成功抓捕一名逃亡者获得的额外积分.
  display_format: "&e[{points}] &r{player}" # 玩家名字在头顶和Tab列表中的显示格式. {player} 玩家名, {points} 总积分.
  ranking_display_count: 5           # 游戏结束时, 排名公告显示前多少名玩家 (设为0或负数则显示所有参与者).
  ranking_broadcast_header: "&6&l游戏结束！最终积分排名：" # 排名公告的标题行.
  ranking_broadcast_format: "&e{rank}. &r{player} &7- &a{points} 分" # 每行排名的格式. {rank}名次, {player}玩家名, {points}积分.

#-------------------------------------------------------------------------------
# Scoreboard Settings - 侧边栏公告榜设置
#-------------------------------------------------------------------------------
scoreboard:
  title: "&6&l猎人游戏"            # 侧边栏公告榜顶部的标题.
  default_on_join: true         # 玩家加入服务器时是否默认开启显示此公告榜.
  footer:                       # 侧边栏公告榜底部显示的自定义信息 (可多行).
    - "&7--------------------"
    - "&eyourserver.com"

#-------------------------------------------------------------------------------
# Game Flow Settings - 游戏流程控制
#-------------------------------------------------------------------------------
game_flow:
  min_runners_to_start: 1       # 开始游戏所需的最少逃亡者数量 (不包括已设定的猎人).
  teleport_on_game_end: true    # 游戏结束时是否将所有参与者传送至 end_game_teleport_location.

#-------------------------------------------------------------------------------
# Hunter Specific Settings - 猎人特定设置
#-------------------------------------------------------------------------------
hunter_settings:
  weapon:                       # 猎人在游戏开始时获得的特殊武器.
    material: "DIAMOND_SWORD"   # 武器的材质类型.
    name: "&c&l狩猎之刃"        # 武器的显示名称.
    lore:                       # 武器的Lore描述.
      - "&4饮血，渴望猎物之魂。"
      - "&7用此利刃终结逃亡！"
  jail_location: "world,0,50,0,0,0" # 备用传送点: 主要用于 spawn_points.spectator 未配置时, 逃亡者被抓捕后的传送地点.
  cannot_drop_weapon_message: "&c作为猎人，你无法丢弃你的狩猎工具！" # 猎人尝试丢弃特殊武器时的提示信息.
```

## 命令详解

所有命令都以 `/hunter` (或别名 `/ht`) 开头。

*   `/hunter`
    *   描述：显示插件的帮助信息，列出可用的子命令。
    *   权限：`hunter.use` (默认所有玩家拥有)

*   `/hunter set <玩家ID>`
    *   描述：将指定的在线玩家设置为猎人。此操作只能在游戏等待阶段 (`WAITING`) 进行。
    *   用法：`/hunter set Notch`
    *   权限：`hunter.admin` (默认OP拥有)

*   `/hunter reset`
    *   描述：强制结束当前游戏（如果正在进行），并重置所有游戏状态、玩家身份和积分。
    *   权限：`hunter.admin`

*   `/hunter npc`
    *   描述：在命令执行者当前所站的位置生成一个任务NPC。NPC的名称等属性由 `config.yml` 中的 `npc_interaction_task` 部分定义。
    *   权限：`hunter.admin`
    *   注意：此命令只能由玩家在游戏内执行。

*   `/hunter start`
    *   描述：手动开始一局新的猎人游戏。需要至少已设置一名猎人，并且在线的非猎人玩家数量达到 `config.yml` 中 `game_flow.min_runners_to_start` 的要求。
    *   权限：`hunter.admin`

*   `/hunter loser`
    *   描述：允许逃亡者玩家在游戏进行阶段（准备或追捕阶段）主动弃权。弃权后将变为旁观者。
    *   权限：`hunter.player` (默认所有玩家拥有)

*   `/hunter king <on|off>`
    *   描述：允许玩家开启或关闭自己屏幕上的侧边栏游戏信息公告榜。
    *   用法：`/hunter king on` 或 `/hunter king off`
    *   权限：`hunter.player`

## 权限节点

*   `hunter.use`: 允许使用 `/hunter` 查看帮助信息。默认所有玩家拥有。
*   `hunter.admin`: 允许使用所有管理员命令 (`/hunter set`, `/hunter reset`, `/hunter npc`, `/hunter start`)。默认OP拥有。
*   `hunter.player`: 允许使用玩家特定命令 (`/hunter loser`, `/hunter king`)。默认所有玩家拥有。

## 注意事项

*   **Citizens 依赖：** 本插件的核心NPC功能依赖于 Citizens 插件。请确保您已正确安装并配置了 Citizens。
*   **配置文件：** 仔细检查 `config.yml` 中的坐标、材质名称等是否正确，错误的配置可能导致插件功能异常。
*   **版本兼容：** 本插件为 Spigot 1.21 开发，请确保您的服务器版本兼容。

---

感谢您使用MyHunterPlugin！

## 高性价比的插件定制
【闲鱼】https://m.tb.cn/h.hbPcilw?tk=hwWaVIoKiVT HU006 「这是我的闲鱼号，快来看看吧～」
点击链接直接打开

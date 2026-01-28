---
tip: 5
title: SGS 合约 Lua 虚拟机上下文规范
description: 详细界定 Taiyi 网络中 SGS 合约所暴露的全局变量、数据结构及 API 函数接口规范。
author: 红中🀄️ (@hongzhongx)
discussions-to: https://github.com/hongzhongx/TIPs/discussions/6
status: Draft
type: Standards Track
category: TRC
created: 2026-01-27
---

## 摘要

本文档详细规范了太乙网络（Taiyi Network）中 SGS（Smart Game Script）合约的 Lua 虚拟机（VM）执行上下文。该规范涵盖了全局变量、全局函数、基础数据结构以及两个核心辅助对象：`contract_helper`（合约助手）和 `nfa_helper`（NFA 助手）。

## 动机

为了确保 SGS 合约在太乙网络节点中的兼容性、安全性及可维护性，需要一套详尽的 API 规范。这不仅有助于“心素”们（开发者）编写高质量的智能合约，也为节点实现提供了统一的标准。

## 规范说明

本文中的关键字"MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY"和"OPTIONAL"均按照RFC 2119和RFC 8174规范解释。

### 1. 全局上下文 (Global Context)

在 SGS Lua 环境中，默认暴露以下全局变量和函数。

### 1.1 全局变量

| 变量名 | 类型 | 说明 |
| :--- | :--- | :--- |
| `contract_base_info` | `table` | 包含当前运行合约的基础元数据。 |
| `contract_helper` | `userdata` | 提供与区块链底层以及太乙网络中各种基础数据交互的通用方法。 |
| `nfa_helper` | `userdata` | 提供与当前绑定 NFA 交互的方法。 |
| `current_contract` | `string` | 当前顶层合约的名称。 |

### 1.2 `contract_base_info` 结构

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| `owner` | `string` | 合约拥有者的账号名。 |
| `name` | `string` | 当前合约的名称。 |
| `caller` | `string` | 当前调用发起者的账号名。 |
| `creation_date` | `string` | 合约创建的虚拟时间戳。 |
| `invoker_contract_name` | `string` | (跨合约调用时) 发起调用的源合约名。 |

### 1.3 全局函数

#### `import_contract(contract_name)`

* **描述**: 导入另一个已发布的合约并返回其导出的 table。
* **参数**: `contract_name` (string) - 目标合约名。
* **返回**: `table` - 目标合约的执行空间环境。

#### `format_vector_with_table(table)`

* **描述**: 将 Lua table 转换为 JSON 字符串，常用于与外部系统交互。
* **参数**: `table` (table) - 需要转换的 table。
* **返回**: `string` - JSON 格式字符串。

---

### 2. 基础数据结构规范 (Basic Data Structures)

本节详细规范了 SGS 合约中所有主要数据结构的属性及其含义。

### `contract_asset_resources` (资产资源结构)

用于描述 NFA 或账号持有的基础建设资源。

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| `gold` | `integer` | 金石数量。 |
| `food` | `integer` | 食物数量。 |
| `wood` | `integer` | 木材数量。 |
| `fabric` | `integer` | 织物数量。 |
| `herb` | `integer` | 药材数量。 |

### `contract_tiandao_property` (天道时间结构)

描述太乙世界内部的虚拟时间系统。

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| `v_years` | `integer` | 虚拟年。 |
| `v_months` | `integer` | 虚拟月。 |
| `v_days` | `integer` | 当前月第几日。 |
| `v_times` | `integer` | 节气编号 (0-23)，对应 24 节气。 |
| `v_timeonday` | `integer` | 当天时刻 (0-3)，对应“凌晨”、“上午”、“下午”、“夜晚”。 |
| `v_1day_blocks` | `integer` | 虚拟一日对应的区块链区块数。 |

### `contract_nfa_base_info` (NFA 基础信息)

描述非同质化资产（Non-Fungible Assets）的核心属性。

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| `id` | `integer` | NFA 的唯一全局 ID。 |
| `symbol` | `string` | NFA 符号，定义了 NFA 的类别。 |
| `min_equivalent_qi` | `integer` | 维持该 NFA 正常运作所需的物质量对应的最低等效真气。 |
| `owner_account` | `string` | 当前拥有该 NFA 的账号。 |
| `creator_account` | `string` | 创建该 NFA 的账号。 |
| `active_account` | `string` | 当前被授权操作该 NFA 的账号。 |
| `qi` | `integer` | NFA 内部积蓄的真气值。 |
| `parent` | `integer` | 父 NFA 的 ID，若为 -1 则无父项。 |
| `five_phase` | `integer` | 五行属性编码。 |
| `main_contract` | `string` | 该 NFA 绑定的主逻辑合约名。 |
| `data` | `table` | 该 NFA 在主合约下的私有状态数据。 |

### `contract_zone_base_info` (区域/空间信息)

描述地理区域或逻辑空间的属性。

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| `nfa_id` | `integer` | 区域对应的 NFA ID。 |
| `name` | `string` | 区域的全局唯一名称。 |
| `type` | `string` | 区域类型名称（如 "城市", "荒野"）。 |
| `type_id` | `integer` | 区域类型的内部 ID。 |
| `ref_prohibited_contract_zone` | `string` | 参考的禁用合约区域映射。 |

### `contract_actor_base_info` (角色基础信息)

描述游戏世界中角色的核心静态与动态状态。

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| `nfa_id` | `integer` | 角色对应的 NFA ID。 |
| `name` | `string` | 角色的完整名称。 |
| `age` | `integer` | 角色当前的虚拟年龄。 |
| `health` | `integer` | 当前健康值。 |
| `health_max` | `integer` | 最大健康值。 |
| `born` | `boolean` | 是否已出生的标志位。 |
| `born_vyears` | `integer` | 出生时的虚拟年。 |
| `born_vmonths` | `integer` | 出生时的虚拟月。 |
| `born_vdays` | `integer` | 出生时的虚拟日。 |
| `born_vtod` | `integer` | 出生时的时刻 (0-3)，对应“凌晨”、“上午”、“下午”、“夜晚”。 |
| `born_vtimes` | `integer` | 出生时的节气。 |
| `five_phase` | `integer` | 角色的五行属性。 |
| `gender` | `integer` | 性别：0 (未知), 1 (男), 2 (女)。 |
| `standpoint` | `integer` | 角色立场取向值。 |
| `standpoint_type` | `integer` | 立场类型的 ID。 |
| `location` | `string` | 当前所在的区域名称。 |
| `base` | `string` | 角色的出生地或归属地。 |
| `init_attribute_amount_max` | `integer` | 出生时初始属性点总数上限。 |
| `talents` | `table` | 角色拥有的天赋规则 ID 列表。 |

### `contract_actor_core_attributes` (角色核心属性)

描述影响角色能力的八项核心数值，每项属性包含当前值和最大值（以 `_max` 后缀区分）。

| 属性 | 含义说明 |
| :--- | :--- |
| `strength` / `strength_max` | **臂力**：影响物理攻击、负重、外家功夫。 |
| `physique` / `physique_max` | **体质**：影响生命值上限、抗性、持久度。 |
| `agility` / `agility_max` | **敏捷**：影响闪避、轻功、出手速度。 |
| `vitality` / `vitality_max` | **根骨**：影响真气承载、内功防御、修炼速度。 |
| `comprehension` / `comprehension_max` | **悟性**：影响功法领悟、技能学习难度。 |
| `willpower` / `willpower_max` | **定力**：影响内功修为、修炼效率。 |
| `charm` / `charm_max` | **魅力**：影响社交互动及好感度获取率。 |
| `mood` / `mood_max` | **心情**：影响行动效率、随机事件触发率。 |

### `contract_actor_talent_rule_info` (天赋规则)

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| `id` | `integer` | 天赋规则的唯一 ID。 |
| `main_contract` | `string` | 天赋逻辑所在的合约。 |
| `title` | `string` | 天赋名称。 |
| `description` | `string` | 天赋的详细文学描述。 |
| `init_attribute_amount_modifier` | `integer` | 对初始分配点数的修正值，会影响角色信息中的`init_attribute_amount_max`值。 |

### `contract_actor_relation_info` (角色关系)

| 属性 | 类型 | 说明 |
| :--- | :--- | :--- |
| `actor_name` | `string` | 角色名称。 |
| `target_actor_name` | `string` | 目标角色名称。 |
| `favor` | `integer` | 原始好感度数值（有正负）。 |
| `favor_level` | `integer` | 好感等级 (-6 到 +6)。 |

---

### 3. 合约助手 API (`contract_helper`)

`contract_helper` 提供了合约层面的系统级操作。

### 系统与叙事 (System & Narrative)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `log(msg)` | `msg`: (string) 消息内容 | `nil` | 在节点控制台打印输出一条系统级日志。主要用于调试。 |
| `narrate(msg, time_prefix)` | `msg`: (string) 叙事文本<br>`time_prefix`: (boolean) 是否显示时间前缀 | `nil` | 输出一段叙事描述，支持颜色标记。若开启时间前缀，将附带当前周期的天道时间。 |
| `make_release()` | 无 | `nil` | 将当前正在开发的合约切换为“已发行”状态，此操作不可逆。 |
| `zuowangdao_account_name()` | 无 | `string` | 获取太乙网络治理层“坐忘道”的全局账号名。 |
| `get_tiandao_property()` | 无 | `table` | 返回包含当前世界天道时间属性的 `contract_tiandao_property` 结构。 |

### 区块链状态 (Blockchain State)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `time()` | 无 | `integer` | 返回当前头区块的 Unix 时间戳 (uint32)。 |
| `block()` | 无 | `integer` | 返回当前区块链的区块高度。 |
| `is_owner()` | 无 | `boolean` | 检查当前账户是否为合约的所有者。 |
| `zuowangdao_account_name()` | 无 | `string` | 获取太乙网络治理层“坐忘道”的全局账号名。 |
| `get_nfa_caller()` | 无 | `integer` | 返回发起此次合约调用的 NFA 实体 ID。若调用非来源于 NFA，则返回 -1。 |

### 随机与数学 (Random & Math)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `random()` | 无 | `integer` | 获取一个高度随机的非负整数。基于当前区块上下文。 |
| `generate_hash(offset)` | `offset`: (integer) 哈希计算偏移量 | `integer` | 生成一个唯一的哈希长整型 (int64)。常用于生成随机 ID 或伪随机种子。 |
| `number_max()` / `number_min()` | 无 | `number` | 返回 Lua 虚拟机支持的最大/最小浮点数值。 |
| `integer_max()` / `integer_min()` | 无 | `integer` | 返回 Lua 虚拟机支持的最大/最小 64 位整型数值。 |
| `hash256(source)` | `source`: (string) 原始内容 | `string` | 对字符串进行 SHA256 哈希计算，返回十六进制结果字符串。 |
| `hash512(source)` | `source`: (string) 原始内容 | `string` | 对字符串进行 SHA512 哈希计算，返回十六进制结果字符串。 |
| `calc_max_equivalent_qi_from_asset(val, sym)` | `val`: (number) 资产数量<br>`sym`: (string) 资产符号 | `integer` | 计算指定数量的资产在当前天道规则下可转化出的最大等效真气量。 |
| `calc_max_equivalent_asset_from_qi(qi, sym)` | `qi`: (integer) 真气值<br>`sym`: (string) 目标资产符号 | `integer` | 计算指定真气值在当前天道规则下可炼化出的最大等效资产数量。 |

### 资产管理 (Asset Management)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `get_account_balance(acc, sym)` | `acc`: (string) 账号名<br>`sym`: (string) 资产符号 | `integer` | 获取指定账号持有的某种资产余额。 |
| `make_memo(to, key, val, ss, log)` | `to`: (string) 接收者<br>`key`: (string) 键<br>`val`: (string) 值<br>`ss`: (integer) 共享密钥<br>`log`: (boolean) 是否记日志 | `table` | 创建一段加密备注信息（memo），常用于转账操作。 |
| `transfer_from_owner(to, val, sym, log)` | `to`: (string) 接收者<br>`val`: (number) 金额<br>`sym`: (string) 符号<br>`log`: (boolean) 是否记日志 | `nil` | 从合约所有者的个人账户通过合约支付资产。 |
| `transfer_from_caller(to, val, sym, log)` | 同上 | `nil` | 从当前调用者的个人账户通过合约支付资产。此操作需调用者在节点层面对合约进行转账授权。 |
| `transfer_nfa_from_owner(to, nfa_id, log)` | `to`: (string) 接收者<br>`nfa_id`: (integer) NFA ID<br>`log`: (boolean) 是否记日志 | `nil` | 将合约所有者持有的指定 NFA 实体转移给其他账户。 |
| `transfer_nfa_from_caller(to, id, log)` | 同上 | `nil` | 将当前调用者持有的指定 NFA 实体转移给其他账户。 |
| `approve_nfa_active_from_owner(to, nfa_id, log)` | `to`: (string) 接收者<br>`nfa_id`: (integer) NFA ID<br>`log`: (boolean) 是否记日志 | `nil` | 授权指定账户作为当前合约所有者持有的 NFA 的 Active 操作者。 |
| `approve_nfa_active_from_caller(to, nfa_id, log)` | 同上 | `nil` | 授权指定账户作为当前调用者持有的 NFA 的 Active 操作者。 |

### 数据存续 (Data Persistence)

合约数据操作通常涉及 `read_list` 或 `write_list` 参数。这些是 Lua table，用于指定状态树中的路径或字段集合。

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `read_contract_data(keys)` | `keys`: (table) 键路径列表 | `table` | 从本合约的全局持久化空间读取数据。 |
| `write_contract_data(data, keys)` | `data`: (table) 原始数据<br>`keys`: (table) 键路径映射 | `nil` | 将数据写入本合约的全局持久化空间。 |
| `read_account_contract_data(keys)` | 同上 | `table` | 读取当前调用者在本合约下的私有或关联数据。 |
| `write_account_contract_data(data, keys)` | 同上 | `nil` | 写入当前调用者在本合约下的私有或关联数据。 |
| `get_contract_data(contract, keys)` | `contract`: (string) 目标合约名<br>`keys`: (table) 键路径列表 | `table` | 读取其他合约明确标记为公共的持久化数据字段。 |
| `get_account_contract_data(acc, ct, ks)` | `acc`: (string) 账号名<br>`ct`: (string) 合约名<br>`ks`: (table) 键路径 | `table` | 读取指定账号在指定合约空间下的关联数据。 |
| `get_contract_source_code(contract)` | `contract`: (string) 合约名 | `string` | 以字符串形式返回指定合约的完整 Lua 源代码。 |
| `invoke_contract_function(ct, fn, json)` | `ct`: (string) 合约名<br>`fn`: (string) 函数名<br>`json`: (string) 参数 JSON | `nil` | 跨合约调用指定合约的函数。参数需预先序列化为 JSON 字符串。 |

### NFA / 角色 / 区域 管理 (Management APIs)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `create_nfa_symbol(sym, desc, contract, max, qi, is_sbt)` | `sym`: (string) 符号<br>`desc`: (string) 描述<br>`contract`: (string) 主合约<br>`max`: (integer) 最大发行量<br>`qi`: (integer) NFA实体正常运转需要的最少等价真气量<br>`is_sbt`: (boolean) 是否不可转让 | `nil` | 注册一种新的 NFA 类型。 |
| `change_nfa_symbol_authority(sym, acc)` | `sym`: (string) 符号<br>`acc`: (string) 授权账号 | `nil` | 变更某种 NFA 符号的创建权限账号。 |
| `change_nfa_symbol_authority_nfa_symbol(sym, auth_sym)` | `sym`: (string) 符号<br>`auth_sym`: (string) 权限 NFA 符号 | `nil` | 设定某种 NFA 的创建权限需持有特定类型的 NFA。 |
| `create_nfa_to_account(acc, sym, data)` | `acc`: (string) 目标账号<br>`sym`: (string) 符号<br>`data`: (table) 初始化数据 | `integer` | 为指定账号创建一个指定类型的 NFA 实例。返回新 NFA 的 ID。发起账号要有创建该类NFA的权限。 |
| `get_nfa_info(id)` | `id`: (integer) NFA ID | `table` | 返回指定 NFA 的基本信息（ `contract_nfa_base_info` 结构）。 |
| `is_nfa_valid(id)` | `id`: (integer) NFA ID | `boolean` | 检查指定的 NFA ID 是否指向一个有效的、未被销毁的实体。 |
| `get_nfa_balance(id, sym)` | `id`: (integer) NFA ID<br>`sym`: (string) 资产符号 | `integer` | 获取指定 NFA 内部持有的某种资产余额。 |
| `get_nfa_resources(id)` | `id`: (integer) NFA ID | `table` | 获取指定 NFA 持有的资源/资产（`contract_asset_resources`）。 |
| `get_nfa_materials(id)` | `id`: (integer) NFA ID | `table` | 获取指定 NFA 内部具备的“材质”资源（`contract_asset_resources`）。 |
| `list_nfa_inventory(id, sym)` | `id`: (integer) NFA ID<br>`sym`: (string) 过滤符号 | `table` | 列出指定 NFA 容器内持有的子 NFA 列表。 |
| `get_nfa_location(id)` | `id`: (integer) NFA ID | `string` | 获取指定 NFA 当前所在的区域/空间名称。 |
| `get_nfa_contract(id)` | `id`: (integer) NFA ID | `string` | 获取指定 NFA 当前绑定的主要逻辑合约名。 |
| `change_nfa_contract(id, ct)` | `id`: (integer) NFA ID<br>`ct`: (string) 目标合约 | `nil` | 变更 NFA 绑定的主要逻辑合约。此操作受各合约权限限制。 |
| `change_nfa_active_operator(id, acc)` | `id`: (integer) NFA ID<br>`acc`: (string) 操作账号 | `nil` | 变更 NFA 的 Active 操作权拥有者。 |
| `read_nfa_contract_data(id, keys)` | `id`: (integer) NFA ID<br>`keys`: (table) 键路径 | `table` | 读取指定 NFA 在其绑定合约下的持久化数据。 |
| `write_nfa_contract_data(id, data, keys)` | `id`: (integer) NFA ID<br>`data`: (table) 数据内容<br>`keys`: (table) 键路径映射 | `nil` | 写入指定 NFA 在其绑定合约下的持久化数据。 |
| `is_nfa_action_exist(id, act)` | `id`: (integer) NFA ID<br>`act`: (string) 行为名 | `boolean` | 检查指定 NFA 是否定义了某种行为指令。 |
| `eval_nfa_action(id, act, params)` | `id`: (integer) NFA ID<br>`act`: (string) 行为名<br>`params`: (table) 参数 | `table` | 只读方式执行 NFA 行为逻辑（eval型行为）。 |
| `do_nfa_action(id, act, params)` | `id`: (integer) NFA ID<br>`act`: (string) 行为名<br>`params`: (table) 参数 | `table` | 上链方式执行 NFA 行为逻辑（do型行为）。 |
| `create_actor(fam, last)` | `fam`: (string) 姓<br>`last`: (string) 名 | `integer` | 创建一个新的角色，返回其绑定的 NFA ID。此时角色尚未“出生”。发起账号要有角色NFA类型实体的创建权限。 |
| `born_actor(name, gender, sexuality, attrs, zone)` | `name`: (string) 全名<br>`gender`: (integer) 性别<br>`sexuality`: (integer) 性向倾向<br>`attrs`: (table) 初始属性分配，所有属性总和不得超过角色信息中的`init_attribute_amount_max`值<br>`zone`: (string) 出生地区域 | `nil` | 执行角色出生逻辑，初始化各项核心属性及地理位置。 |
| `move_actor(actor, zone)` | `actor`: (string) 角色名称<br>`zone`: (string) 目标区域名 | `nil` | 将角色移动到另一个区域。目标区域必须和角色当前区域有路径连接。 |
| `create_zone(name, type)` | `name`: (string) 区域名<br>`type`: (string) 区域类型 | `integer` | 创建一个地理区域，返回其 NFA ID。发起账号要有区域NFA类型实体的创建权限（心素）。 |
| `change_zone_type(id, type)` | `id`: (integer) 区域 NFA ID<br>`type`: (string) 新类型 | `nil` | 变更区域的类型定义。 |
| `refine_zone(id)` | `id`: (integer) 区域 NFA ID | `string` | 对区域进行“炼化”操作，根据其中投入的材质重新生成区域特性。 |
| `connect_zones(z1, z2)` | `z1`: (integer) 区域 1 ID<br>`z2`: (integer) 区域 2 ID | `nil` | 建立从区域1到区域2的单向路径连接。注意参数为 NFA ID。 |
| `list_actors_on_zone(id)` | `id`: (integer) 区域 NFA ID | `table` | 获取当前所在该区域的所有角色信息列表。 |
| `exploit_zone(actor, zone)` | `actor`: (string) 角色名<br>`zone`: (string) 区域名 | `string` | 指派角色对指定区域进行探索或采集。 |
| `is_contract_allowed_by_zone(zone, ct)` | `zone`: (string) 区域名<br>`ct`: (string) 合约名 | `boolean` | 检查指定合约在特定区域是否被允许执行。 |
| `set_zone_contract_permission(z, c, a)` | `z`: (string) 区域名<br>`c`: (string) 合约名<br>`a`: (boolean) 是否允许 | `nil` | 设置区域是否允许特定合约在本区域上运行。 |
| `remove_zone_contract_permission(z, c)` | `z`: (string) 区域名<br>`c`: (string) 合约名 | `nil` | 移除区域对特定合约的运行禁令。 |
| `set_zone_ref_prohibited_contract_zone(z, r)` | `z`: (string) 区域名<br>`r`: (string) 参考区域 | `nil` | 设置区域禁用的合约引用参照区域。 |
| `is_zone_valid(id)` | `id`: (integer) NFA ID | `boolean` | 检查指定的 ID 是否为有效区域实体。 |
| `is_zone_valid_by_name(name)` | `name`: (string) 区域名 | `boolean` | 检查指定名称的区域是否存在。 |
| `get_zone_info(id)` | `id`: (integer) 区域 NFA ID | `table` | 返回 `contract_zone_base_info` 结构。 |
| `get_zone_info_by_name(name)` | `name`: (string) 区域名 | `table` | 根据名称返回 `contract_zone_base_info` 结构。 |
| `create_actor_talent_rule(ct)` | `ct`: (string) 合约名 | `integer` | 注册一种新的角色天赋规则。 |
| `is_actor_valid(id)` | `id`: (integer) NFA ID | `boolean` | 检查指定的 ID 是否为有效角色实体。 |
| `is_actor_valid_by_name(name)` | `name`: (string) 角色名 | `boolean` | 检查指定名称的角色是否存在。 |
| `get_actor_info(id)` | `id`: (integer) 角色 ID | `table` | 返回 `contract_actor_base_info` 结构。 |
| `get_actor_info_by_name(name)` | `name`: (string) 角色名 | `table` | 根据名称返回 `contract_actor_base_info` 结构。 |
| `get_actor_attributes(id)`| `id`: (integer) 角色 ID | `table` | 返回 `contract_actor_core_attributes` 结构。 |

### 修真管理 (Cultivation Management)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `create_cultivation(id, ids, shr, t)` | `id`: (integer) 管理者 NFA ID<br>`ids`: (table) 受益者 NFA ID 列表<br>`shr`: (table) 受益比例列表<br>`t`: (integer) 准备时间（区块数） | `integer` | 发起一项修真活动。返回修真活动 ID。 |
| `participate_cultivation(cid, id, val)` | `cid`: (integer) 修真 ID<br>`id`: (integer) 参与者 NFA ID<br>`val`: (integer) 投入真气量 | `string` | 让指定 NFA 参与到修真活动中。返回错误信息（如有）。 |
| `start_cultivation(id)` | `id`: (integer) 修真 ID | `string` | 正式开启已准备就绪的修真活动。 |
| `stop_and_close_cultivation(id)` | `id`: (integer) 修真 ID | `string` | 停止并结算修真奖励，随后关闭活动。 |
| `update_cultivation(id)` | `id`: (integer) 修真 ID | `string` | 手动更新修真活动的进度状态。 |
| `is_cultivation_exist(id)` | `id`: (integer) 修真 ID | `boolean` | 检查指定的修真活动是否存在。 |

### 治理与提案 (Governance & Proposals)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `create_named_contract(name, code)` | `name`: (string) 合约名<br>`code`: (string) 源代码 | `nil` | 由“心素”直接在天道层面注册并部署一个具名合约。 |
| `create_proposal(ct, fn, ps, sb, et)` | `ct`: (string) 目标合约名<br>`fn`: (string) 目标函数名<br>`ps`: (table) 参数列表<br>`sb`: (string) 提案主题<br>`et`: (integer) 截止时间（Unix 时间戳） | `integer` | 发起一项治理提案。返回提案 ID。 |
| `update_proposal_votes(pids, app)` | `pids`: (table) 提案 ID 列表<br>`app`: (boolean) 是否赞成 | `nil` | 对一组提案进行投票表态。 |
| `remove_proposals(pids)` | `pids`: (table) 提案 ID 列表 | `nil` | 撤回或移除指定的提案（仅限发起者）。 |
| `grant_xinsu(acc)` | `acc`: (string) 账号名 | `nil` | 授予指定账号“心素”权限（治理开发者权限）。 |
| `revoke_xinsu(acc)` | `acc`: (string) 账号名 | `nil` | 撤销指定账号的“心素”权限。 |

---

### 4. NFA 助手 API (`nfa_helper`)

`nfa_helper` 始终隐式绑定到当前正在操作的 NFA 对象。

### 状态检索 (State Retrieval)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `get_info()` | 无 | `table` | 返回当前 NFA 的 `contract_nfa_base_info` 结构。 |
| `get_resources()` | 无 | `table` | 获取当前 NFA 持有的资源/资产（`contract_asset_resources`）。 |
| `get_materials()` | 无 | `table` | 获取当前 NFA 内部具备的“材质”资源（`contract_asset_resources`）。 |

### 生命周期与回调 (Lifecycle)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `enable_tick()` | 无 | `nil` | 开启 NFA 的心跳。开启后，每隔一段时间后将自动触发合约内的 `on_heart_beat` 函数。太乙网络会在最小100息之后（大约5分钟），就会触发一次 NFA 的 `on_heart_beat`。 |
| `disable_tick()` | 无 | `nil` | 暂停 NFA 的心跳回调。 |
| `destroy()` | 无 | `nil` | 永久销毁该 NFA。本质上只是将NFA的所有者和操作者权限置空。 |

### 真气与跨层管理 (Qi & Resource)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `convert_qi_to_resource(val, sym)` | `val`: (integer) 真气量<br>`sym`: (string) 目标资源的符号 | `nil` | 将 NFA 的真气转化为资源资产并存入NFA。 |
| `convert_resource_to_qi(val, sym)` | `val`: (integer) 资源量<br>`sym`: (string) 资源符号 | `nil` | 将NFA内的资源炼化为真气并积蓄。 |
| `transfer_to(id, val, sym, log)` | `id`: (integer) 目标NFA ID<br>`val`: (number) 资源数量<br>`sym`: (string) 资源符号<br>`log`: (boolean) 是否记日志 | `nil` | 在两个 NFA 之间直接划转基础资源. |
| `inject_material_to(id, val, sym, log)` | `id`: (integer) 目标NFA ID<br>`val`: (number) 资源数量<br>`sym`: (string) 资源符号<br>`log`: (boolean) 是否记日志 | `nil` | 将本 NFA 的资源注入另一个 NFA 作为其“材质”组成部分。 |
| `separate_material_out(id, val, sym, log)` | `id`: (integer) 目标NFA ID<br>`val`: (number) 资源数量<br>`sym`: (string) 资源符号<br>`log`: (boolean) 是否记日志 | `nil` | 从自身材质中提取特定资源剥离并转移给目标 NFA。 |
| `deposit_from(acc, val, sym, log)` | `acc`: (string) 账号<br>`val`: (number) 资源数量<br>`sym`: (string) 资源符号<br>`log`: (boolean) 是否记日志 | `nil` | 从外部个人账号通过合约向此 NFA 存入资源。 |
| `deposit_from_owner(val, sym, log)` | `val`: (number) 资源数量<br>`sym`: (string) 资源符号<br>`log`: (boolean) 是否记日志 | `nil` | 从合约所有者的个人账户向此 NFA 存入资源。 |
| `withdraw_to(acc, val, sym, log)` | `acc`: (string) 账号<br>`val`: (number) 资源数量<br>`sym`: (string) 资源符号<br>`log`: (boolean) 是否记日志 | `nil` | 将此 NFA 内部资源提取到外部个人账号。 |

### 层级管理 (Hierarchy Management)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `add_child(id)` | `id`: (integer) NFA ID | `nil` | 将指定 NFA 纳入为本 NFA 的子实体（如：装备到角色身上）。 |
| `add_to_parent(id)` | `id`: (integer) NFA ID | `nil` | 将本 NFA 挂载到指定父 NFA 实体。 |
| `add_child_from_contract_owner(id)` | `id`: (integer) NFA ID | `nil` | 以合约所有者身份将其持有的 NFA 纳入为本 NFA 的子实体。 |
| `add_to_parent_from_contract_owner(id)` | `id`: (integer) NFA ID | `nil` | 以合约所有者身份将本 NFA 挂载到其持有的父 NFA 实体。 |
| `remove_from_parent()` | 无 | `nil` | 脱离当前的父子关联链，变回独立存在。 |

### NFA 数据存续与行为 (Data & Actions)

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `read_contract_data(keys)` | `keys`: (table) 键路径 | `table` | 读取专属于该 NFA 实例在该合约下的持久化数据。 |
| `write_contract_data(d, k)` | `d`: (table) 原始数据<br>`k`: (table) 键路径 | `nil` | 写入该 NFA 专有的合约数据。 |
| `eval_action(name, params)` | `name`: (string) 行为名<br>`params`: (table) 参数 | `table` | 执行 NFA 的只读行为并返回结果。 |
| `do_action(name, params)` | `name`: (string) 行为名<br>`params`: (table) 参数 | `table` | 执行 NFA 的行为并返回结果。 |
| `create_nfa_to_actor(at, s, d)` | `at`: (integer) 目标角色 NFA ID<br>`s`: (string) NFA 符号<br>`d`: (table) 数据 | `integer` | 为指定角色创建一个新的 NFA 附属品（成为目标NFA的子实体，如装备到角色身上）。 |
| `create_nfa_to_account(acc, sym, data)` | `acc`: (string) 目标账号<br>`sym`: (string) NFA 符号<br>`data`: (table) 数据 | `integer` | 为指定账号创建一个新的独立 NFA 实例。 |

### 角色互动专有 (Actor Exclusive)

仅当当前 NFA 为角色类型（Actor）时有效。

| 函数签名 | 参数说明 | 返回值 | 详细说明 |
| :--- | :--- | :--- | :--- |
| `modify_actor_attributes(v, m)` | `v`: (table) 当前值修正<br>`m`: (table) 上限修正 | `nil` | 直接调整角色的臂力、根骨等核心属性数值。 |
| `get_actor_relation_info(target)` | `target`: (string) 目标角色名 | `table` | 获取与目标角色的关系信息（`contract_actor_relation_info`）。 |
| `modify_actor_relation_values(target, vals)` | `target`: (string) 目标角色名称<br>`vals`: (table) 修正值 | `nil` | 调整自己对指定角色的好感度/关系量化值。 |
| `talk_to_actor(target, text)` | `target`: (string) 目标角色名称<br>`text`: (string) 文本内容 | `nil` | 与目标角色进行文本交互，常用于触发对话事件。 |
| `get_actor_talent_trigger_number(id)` | `id`: (integer) 天赋 ID | `integer` | 查询指定天赋当前已经被触发的次数。 |
| `set_actor_talent_trigger_number(id, n)` | `id`: (integer) 天赋 ID<br>`n`: (integer) 数值 | `nil` | 设置天赋的触发计数（如重置冷却）。 |

---

## 基本原理

本规范的设计目标是提供一个高性能、确定性且易于使用的合约开发环境。

* **contract_helper**：被设计为系统级单例，用于处理与本合约和全局链状态相关的非 NFA 绑定操作。
* **nfa_helper**：作为一个隐式上下文对象（Context-bound object），它简化了 NFA 属性操作的复杂度，使得合约代码更聚焦于业务逻辑而非繁琐配件 ID 定位。
* **数据隔离**：通过 `read/write_contract_data` 等方法，强制实现了基于 NFA 或 Account 的两级存储隔离，确保了多租户环境下的数据安全性。

## 向后兼容性

本规范基于当前太乙节点（Taiyi Node）的主线实现版本。

* **API 稳定性**：现有 API 签名在 `v1.0.0` 终版发布前可能发生微调，但会保持语义一致性。
* **扩展性**：新增的底层能力将通过向 `contract_helper` 和 `nfa_helper` 注册新方法来实现，不影响旧合约的导入和执行。

## 测试用例

测试用例应涵盖以下核心场景：

1. **资产划转验证**：验证 `contract_helper:transfer_from_caller` 在无授权情况下的失败表现。
2. **角色属性修正**：验证 `nfa_helper:modify_actor_attributes` 对 `strength_max` 的溢出截断逻辑。
3. **跨合约调用隔离**：验证 `invoke_contract_function` 无法越权访问源合约的私有持久化空间。

## 参考实现

参考实现可参见太乙合约用例仓库中的：

* [凡人角色主合约（Normal Actor Template）](https://github.com/hongzhongx/taiyi-contracts/blob/main/nfas/actors/normal.lua)

## 安全性注意事项

在编写 SGS 合约时，开发者必须注意以下安全限制：

1. **确定性限制**：严禁使用 `os.time()` 或外部 IO 库，必须通过 `contract_helper:time()` 获取受共识保护的时间。
2. **真气溢出风险**：在进行大量物理资源与真气转换时，务必检查不同资源的精度与整型转换的边界条件。
3. **权限劫持防御**：绝不应仅依赖 `nfa_helper` 绑定的上下文来判断所有权，核心资产操作应显式校验 `contract_base_info.caller`。

## 版权许可

依据[CC0](../LICENSE)协议放弃所有版权及相关权利。

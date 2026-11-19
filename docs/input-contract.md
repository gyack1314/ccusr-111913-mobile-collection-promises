# 以车代库的需求风险试算与服务承诺分析资料契约

以下为项目本地合成资料，不代表真实居民、机构、金额或政策。所有数据为 UTF-8 JSON Lines，每行是一个完整对象；金额采用整数分，null 表示未知或不适用，不能自动等同于零。时间戳含 +08:00 偏移，日期为当地业务日。known_at 或 reported_at 控制资料何时可得，业务发生时间不意味着当时已经获知。

## 业务含义

重量kg和体积m3分别控制，不能用固定密度互换。历史stops的loaded_kg缺失表示未观察，uncollected_kg缺失表示未知未收量，二者不推导相等。现场loaded按预约引用和实际类别作用。车班08:00至18:00；节点路段缺观测可用有说明的保守情景，不得按0分钟。暂存起点为物料实际装车时刻，只有适用品类在指定卸料点完成卸料才结束；卸料点max_kg在当日所有车辆间共享。bookings.confirmed为先前已承诺服务，不等同于应用新接受计划，后续调整需区分原承诺与居民新确认。

## 原始记录

### bookings

| 字段 | 含义 |
| --- | --- |
| id | 预约身份 |
| home | 合成居民 |
| node | 停靠地点 |
| category | 品类 |
| weight_low_kg | 重量下界 |
| weight_high_kg | 重量上界 |
| volume_low_m3 | 体积下界 |
| volume_high_m3 | 体积上界 |
| day | 服务日 |
| window_start | 窗口起点 |
| window_end | 窗口终点 |
| confirmed | 已形成居民承诺 |
| flexible_days | 允许改约范围 |

### travel

| 字段 | 含义 |
| --- | --- |
| id | 路段观察 |
| from_node | 出发点 |
| to_node | 到达点 |
| day | 发生日 |
| weather | 共同天气条件 |
| minutes | 实际分钟 |
| known_at | 记录获知时间 |
| fold | 独立日期核对范围 |

### stops

| 字段 | 含义 |
| --- | --- |
| id | 停靠观察 |
| node | 地点 |
| day | 发生日 |
| booked_kg | 已约量 |
| loaded_kg | 实装量，缺回执为空 |
| uncollected_kg | 已知未收量，未观察为空 |
| minutes | 实际停留 |
| status | 完成部分取消或未遇 |
| weather | 共同天气 |
| known_at | 到达时间 |

### vehicles

| 字段 | 含义 |
| --- | --- |
| id | 车辆 |
| weight_kg | 载重上限 |
| volume_m3 | 体积上限 |
| categories | 允许品类 |
| incompatible_pairs | 不能混载的品类对 |
| max_storage_minutes | 分品类最长在途暂存 |
| start_node | 起点 |
| shift_start | 开班 |
| shift_end | 结束 |
| cost_cents | 班次成本 |

### destinations

| 字段 | 含义 |
| --- | --- |
| id | 接收点窗口 |
| node | 地点 |
| day | 接收日 |
| categories | 接收品类 |
| open | 开门 |
| close | 关门 |
| max_kg | 该窗口共享最大接收量 |
| unload_minutes | 每车卸料时间 |
| cost_cents_per_kg | 处理单价 |

### live_events

| 字段 | 含义 |
| --- | --- |
| id | 现场回执 |
| kind | 停靠装车或接收点变更 |
| target | 作用预约或窗口 |
| occurred_at | 发生时间 |
| known_at | 到达时间 |
| payload | 对应实际字段 |


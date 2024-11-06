# 单一钱包接口

- 目录
  + [1 接口列表](#1-----)
    + [1.1 玩家余额查询](#11-----)
      + [1.1.1 传入参数](#111-----)
      + [1.1.2 返回参数](#112-----)
    + [1.2 请求余额更新](#12-----)
      + [1.2.1 传入参数](#121-----)
      + [1.2.2 返回参数](#122-----)
  + [2 附件](#2-----)
    + [2.1 资金变动类型action_type](#21-----)
    + [2.2 错误代码error_code](#22-----)

### <span id="1-----">1 接口列表</span>
<br>
<span id="11-----">1.1 玩家余额查询</span>

Header：`Content-Type: application/json;charset=utf-8`
<br>
Endpoint: `/api-v1/balance-check`

##### <span id="111-----">1.1.1 传入参数</span>

| 参数名      | 必填 | 类型    | 字段长度 | 例子     | 说明        |
| ----------- | ---- | ------- | -------- | -------- | ------------------------ |
| member_id   | 是   | string     | 50        | YK398362287        | 渠道玩家ID |
| timestamp  |  是   | string  | 10      |  1730456083   | 时间戳  |
| sign  |  是   | string  | 32    |  827ccb0eea8a706c4c34a16891f84e7b  | md5(transaction_id+member_id+action_value+action_type+timestamp+密钥)    |

##### <span id="112-----">1.1.2 返回参数</span>

| 参数名 | 类型   | 字段长度 | 例子    | 说明      |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| status    | int | 1  |    1     | 查询结果，1=成功，2=失败 |
| balance    | decimal | 18,2   | 8002.00 | 玩家余额   |
| error_code    | string | 32      |  TIME_OUT  | 错误代码，详见[附件2.2错误代码](#22-----)描述  |

#### <span id="12-----">1.2 请求余额更新</span>

Header：`Content-Type: application/json;charset=utf-8`
<br>
Endpoint: `/api-v1/balance-update`

##### <span id="121-----">1.2.1 传入参数</span>

| 参数名      | 必填 | 类型    | 字段长度 | 例子     | 说明                     |
| ----------- | ---- | ------- | -------- | -------- | ------------------------ |
| transaction_id       | 是   | string  | 32       |     23985235     | 交易流水号       |
| member_id   | 是   | string     | 50        | YK398362287        | 渠道玩家ID |
| action_value     | 是   | decimal | 18,2    | 820.00  | 资金变动的金额，正数代表增加，负数代表扣减，例如100代表余额增加100，-200代表余额扣减200  |
| action_type | 是   | int | 2    | 1  | 资金变动类型，详见[附件2.1资金变动类型](#21-----)描述      |
| order_id |    | string | 50    | S03827836  | 订单号，如涉及投注订单则传入订单号；如果不涉及订单，可能为空    |
| remarks |    | string | 100    | 01034388769  | 备注，如资金变动类型为余额调整则为空   |
| timestamp  |   是   | string  | 10      |  1730456083   | 时间戳  |
| sign  |  是   | string  | 32    |  827ccb0eea8a706c4c34a16891f84e7b  | md5(transaction_id+member_id+action_value+action_type+timestamp+密钥)     |

##### <span id="122-----">1.2.2 返回参数</span>

| 参数名 | 类型   | 字段长度 | 例子    | 说明                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| transaction_id         | string  | 32      |  23985235     | 交易流水号       |
| status    | int | 1  |    1     | 更新结果，1=成功，2=失败 |
| balance    | decimal | 18,2   | 8002.00 | 玩家余额，如果成功返回最新余额，如果失败返回当前余额   |
| error_code    | string | 32      |  OUT_OF_BALANCE  | 错误代码，详见[附件2.2错误代码](#22-----)描述  |

### <span id="2-----">2 附件</span>

#### <span id="21-----">2.1 资金变动类型action_type</span>

| 参数   | 资金变动类型     |
| ---- | -------- |
| 1    | 投注本金扣除 |
| 2    | 投注本金返还 |
| 3    | 派彩 |
| 4    | 其他余额调整 |

#### <span id="22-----">2.2 错误代码error_code</span>

| 参数   | 代码含义     |
| ---- | -------- |
| OUT_OF_BALANCE | 余额不足 |
| TIME_OUT | 超时 |
| OTHERS  | 其他情况 |

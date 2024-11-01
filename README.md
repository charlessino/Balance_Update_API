# 单一钱包接口

- 目录
  + [1 接口列表](#1-----)
    + [1.1 请求余额更新](#11-----)
      + [1.1.1 传入参数](#111-----)
      + [1.1.2 返回参数](#112-----)
  + [2 附件](#2-----)
    + [2.1 action_type](#21-----)
    + [2.2 error_code](#22-----)

### <span id="1-----">1 接口列表</span>


#### <span id="11-----">1.1 请求余额更新</span>

Header：Content-Type: application/json;charset=utf-8

##### <span id="111-----">1.1.1 传入参数</span>

| 参数名      | 必填 | 类型    | 字段长度 | 例子     | 说明                     |
| ----------- | ---- | ------- | -------- | -------- | ------------------------ |
| transaction_id       | 是   | string  | 32       |          | 交易流水号       |
| member_id   | 是   | string     | 50        | YK398362287        | 渠道玩家ID |
| action_value     | 是   | decimal | 18,2    | 820.00  | 资金变动的金额，正数代表增加，负数代表扣减，例如100代表余额增加100，-200代表余额扣减200  |
| action_type | 是   | int | 2    | 1  | 资金变动类型，详见[附件2.1资金变动类型](#21-----)描述      |
| order_id |    | string | 50    | S03827836  | 如涉及投注订单，这里会传订单号；如果不涉及订单，可能为空    |
| remarks |    | string | 100    | 01034388769  | 备注，可能为空   |
| timestamp  |      | string  | 10      |  1730456083   | 时间戳  |
| sign  |  是   | string  | 32    |  827ccb0eea8a706c4c34a16891f84e7b  | md5(a+b+c+d+g+密钥)作为签名，确认请求的合法性     |

##### <span id="112-----">1.1.2 返回参数</span>

| 参数名 | 类型   | 字段长度 | 例子    | 说明                                         |
| ------ | ------ | -------- | ------- | -------------------------------------------- |
| transaction_id       | 是   | string  | 32       |          | 流水号，方便双方核对记录       |
| status    | int | 1  |    1     | 更新结果，1=成功，2=失败 |
| balance    | decimal | 18,2   | 8002.00 | 玩家余额，如果成功返回最新余额，如果失败返回当前余额   |
| error_code    | string | 32      |    | 错误代码，详见附件2.2错误代码描述  |
| outOrderId    | string | 100      |  OUT_OF_BALANCE  | 商户订单号 |




### <span id="2-----">2 附件</span>

#### <span id="21-----">2.1 资金变动类型</span>

| 参数   | 资金变动类型     |
| ---- | -------- |
| 1    | 玩家下单 |
| 2    | 订单赢利 |
| 3    | 订单取消 |
| 4    | 手动调整 |

#### <span id="22-----">2.2 错误代码</span>

| 参数   | 代码含义     |
| ---- | -------- |
| OUT_OF_BALANCE | 余额不足 |
| OTHERS  | 其他情况 |

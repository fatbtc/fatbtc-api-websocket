# FatBTC websocket Api 示例

Api Docs in English: https://github.com/fatbtc/fatbtc-api-websocket/blob/master/README_en.md

## 注意事项

- 1.Url
wss://www.fatbtc.com/websocket (中国大陆请使用wss://www.fatbtc.us/websocket)

- 2.返回数据:

		所有接口返回数据格式都为  {"status":1, "msg":"success", data:""}
		
		1. status: 状态码, 1表示成功 其他表示失败 
		
		2. msg/message: 提示信息   
            ILLEGAL_WEBSOCKET_KEY 非法key值
            ILLEGAL_WEBSOCKET_MESSAGE 非法消息


## 接口说明

- 1.连接: 
          
          url: wss://www.fatbtc.us/websocket 
          
          连接成功返回：{"msg":"success", "status":1}

- 2.K线: 
          
          订阅:  
          {key:kline_$symbol_$type, ts:$timestamp} 
          $symbol: 交易对
          $type: 时间单位(1min, 5min, 15min, 30min, 60min, 1day, 1mon, 1week, 1year)
          $timestamp: 时间戳 
          
          数据说明: 
          首次订阅成功返回100条，然后每秒返回一条
          {
            "data":{
              "datas":[[时间戳, 开盘价, 最高价, 最低价, 收盘价, 成交数量, 成交笔数, 成交额], ...],
              "msg":"success",
              "status":1,
              "symbol":交易对 同$symbol,
              "timestamp": 时间戳,
              "type":时间单位 同$type
            },
            "key": kline_$symbol_$type
          }
          
          取消: 
          {"key":"kline_cancel", "ts":$timestamp}
          


- 2.K线历史数据: 
          
          订阅:  
          {key:hiskline_$symbol_$type_$timestamp_start, ts:$timestamp} 
          
          参数说明: 
          $symbol: 交易对
          $type: 时间单位(1min, 5min, 15min, 30min, 60min, 1day, 1mon, 1week, 1year)
          $timestamp_start: 起始时间戳，一般情况传现有K线时间最早的那个时间戳，从起始timestamp(不包含start_timestamp这条)往更早的时间返回100条数据
          $timestamp: 时间戳 
          
          数据说明: 
          请求成功返回100条
          {
            "data":{
              "datas":[[时间戳, 开盘价, 最高价, 最低价, 收盘价, 成交数量, 成交笔数, 成交额], ...],
              "msg":"success",
              "status":1,
              "symbol":交易对 同$symbol,
              "timestamp": 时间戳,
              "type":时间单位 同$type
            },
            "key": "kline_$symbol_$type"
          }
          

- 3.市场深度(买卖盘): 
          
          订阅:  
          {key:depth_$symbol_$step, ts:$timestamp} 
          $symbol: 交易对
          $step: 0
          $timestamp: 时间戳 
          
          数据说明: 
          {
            "data":{
              "asks":[
                卖方报价[价格1, 数量1], [价格2, 数量2], ...
              ],
              "bids":[
                买方报价[价格1, 数量1], [价格2, 数量2], ...
              ],
              "msg":"success",
              "status":1,
              "symbol":交易对,
              "timestamp":时间戳 
            },
            "key":"depth_btcfcny_0"
          }
          
          取消: 
          {"key":"depth_$symbol_$step_cancel", "ts":$timestamp}
          


- 4.最新成交: 
          
          订阅:  
          {key:trades_$symbol_$size, ts:$timestamp} 
          $symbol: 交易对
          $size: 数据条数，取值1, 10, 20, 50
          $timestamp: 时间戳 
          
          数据说明: 
          {
            "data":{
              "msg":"success",
              "status":1,
              "symbol":"btcfcny",
              "timestamp":1535452091509,
              "trades":[
                {
                  "price":成交价,
                  "taker": 吃单方 buy/sell 买/卖 ,
                  "timestamp":时间戳,
                  "upOrDown":相对于最近一次价格的涨跌，1涨，-1跌,
                  "volume":成交数量
                },
                ...
                ]
              },
              "key":trades_$symbol_$size
            }
          
          取消: 
          {"key":"trades_$symbol_$size_cancel", "ts":$timestamp}
    



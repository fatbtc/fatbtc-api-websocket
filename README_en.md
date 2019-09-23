# FatBTC Websocket API Example

## Notes

- 1.Url
wss://www.fatbtc.com/websocket (For developers in mainland China, please access wss://www.fatbtc.us/websocket)

- 2.Returned data:

		The data format returned by all interfaces is  {"status":1, "msg":"success", data:""}
		
		1. status: Status code, 1 means success and other means failures 
		
		2. msg/message: Prompt message   
            ILLEGAL_WEBSOCKET_KEY illegal key value
            ILLEGAL_WEBSOCKET_MESSAGE Illegal news


## Interface Description

- 1.connect: 
          
          url: wss://www.fatbtc.us/websocket 
          
          connection successfully returned：{"msg":"success", "status":1}

- 2.K line: 
          
          Subscribe:  
          {key:kline_$symbol_$type, ts:$timestamp} 
          $symbol: trading pair
          $type: time unit(1min, 5min, 15min, 30min, 60min, 1day, 1mon, 1week, 1year)
          $timestamp: timestamp          
          data description: 
          100 K lines are returned on the first successful subscription, and then one per second
          {
            "data":{
              "datas":[[timestamp, opening price, highest price, lowest price, closing price, executed amount, number of transactions, volume], ...],
              "msg":"success",
              "status":1,
              "symbol":traing pair same as$symbol,
              "timestamp": timestamp,
              "type":time unit same as$type
            },
            "key": kline_$symbol_$type
          }
          
          cancel: 
          {"key":"kline_cancel", "ts":$timestamp}
          


- 2.K line historical data: 
          
          subscribe:  
          {key:hiskline_$symbol_$type_$timestamp_start, ts:$timestamp} 
          
          parameter description: 
          $symbol: trading pair         $type: time unit(1min, 5min, 15min, 30min, 60min, 1day, 1mon, 1week, 1year)
          $timestamp_start: the start timestamp, in general, the timestamp of the earliest K-line time, returning 100 entries of data from the start timestamp (excluding start_timestamp) to an earlier time.         $timestamp: timestamp 
          
          data description: 
          return 100 entries after a successful request          {
          "data":{
              "datas":[[timestamp, opening price, opening price, highest price, lowest price, closing price, executed amount, number of transactions, volume], ...],
              "msg":"success",
              "status":1,
              "symbol":trading pair same as$symbol,
              "timestamp": timestamp,
              "type":time unit same as$type
            },
            "key": "kline_$symbol_$type"
          }
          

- 3.Market depth(orders): 
          
          subscription:  
          {key:depth_$symbol_$step, ts:$timestamp} 
          $symbol: trading pair
          $step: 0
          $timestamp: timestamp 
          
          data description: 
          {
            "data":{
              "asks":[
                seller's quotation [price 1, quantity 1], [price 2, quantity 2], ...
              ],
              "bids":[
                buyer's quotation [price 1, amount 1], [price 2, amount 2], ...
              ],
              "msg":"success",
              "status":1,
              "symbol":trading pair,
              "timestamp":timestamp 
            },
            "key":"depth_btcfcny_0"
          }
          
          cancel: 
          {"key":"depth_$symbol_$step_cancel", "ts":$timestamp}
          


- 4.latest transactions: 
          
          subscription:  
          {key:trades_$symbol_$size, ts:$timestamp} 
          $symbol: trading pair
          $size: number of data, value 1, 10, 20, 50          $timestamp: timestamp 
          
          data description: 
          {
            "data":{
              "msg":"success",
              "status":1,
              "symbol":"btcfcny",
              "timestamp":1535452091509,
              "trades":[
                {
                  "price":executed price,
                  "taker": taker buy/sell buy/sell ,
                  "timestamp":timestamp,
                  "upOrDown":relative to the last time the price went up or down, 1 is up and -1 is down,
                  "volume":executed amount               },
                ...
                ]
              },
              "key":trades_$symbol_$size
            }
          
          cancel: 
          {"key":"trades_$symbol_$size_cancel", "ts":$timestamp}
    



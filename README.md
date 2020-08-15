# 一、消息格式

* 请求

  ```json
  {// 0xAA
      "id": 0x0000, 	// 消息ID，2个字节
      "type": 0x00,	// 请求
      "cmd": 0x00,	// 0x00 - 0xFF
      "length": 0,	// 指定data的长度
      "data": {		// 参数
      },
      "crc": 0x00     // 校验值 = crc(id, type, cmd, length, data)，１个字节
  }// 0x55
  ```

* 成功应答

  ```json
  {
      "id": 0x00, 	// 消息ID
      "type": 0x01,   // 应答：成功
      "cmd": 0x00,	// 0x00 - 0xFF
      "length": 0,	// 指定data的长度
      "data": {		// 参数
      }
  }
  ```

* 出错应答

  ```json
  {
      "id": 0x00, 	// 消息ID
      "type": 0x02,   // 应答：出错了
      "cmd": 0x00,	// 0x00 - 0xFF
      "data": {
          "error": 0x00 // 错误代码
      }
  }
  ```
  
  
  
* 通知（暂时不用）

  ```json
  {
      "id": 0x00, 	// 消息ID
      "type": 0x03,   // 通知，无应答
      "cmd": 0x00,	// 0x00 - 0xFF
      "length": 0,	// 指定data的长度
      "data": {		// 参数
      }
  }
  ```



## 1. 关闭设备

* 请求

  | 字段 | data |
  | ---- | ---- |
  | cmd  | 0x00 |
  | data | 无   |

*  应答

  | 字段 | data |
  | ---- | ---- |
  | cmd  | 0x00 |
  | data | 无   |
  
  

## 2. 启动设备

* 请求

  | 字段 | data |
  | ---- | ---- |
  | cmd  | 0x01 |
  | data | 无   |

* 应答

  | 字段 | data |
  | ---- | ---- |
  | cmd  | 0x01 |
  | data | 无   |


## 3. 获取设备列表

* 请求

  | 字段 | data |
  | ---- | ---- |
  | cmd  | 0x02 |
  | data | 无   |

* 应答

  | 字段 | data |
  | ---- | ---- |
  | cmd  | 0x02 |
  | data |      |

  * data

    ```json
    [
       {
           "address": 0x00,		// 设备地址，一个字节
           "device-type": 0x08, 	// 设备类型（开关），一个字节
       },
       {
           "address": 0x01,		// 设备地址，一个字节
           "device-type": 0x05, 	// 设备类型（灯），一个字节
       }
    ]
    ```



## 4. 读取设备属性值

* 请求

  | 字段 | data |
  | ---- | ---- |
  | cmd  | 0x03 |
  | data |      |

  * data

    ```json
    {
        "address": 0x07
    }
    ```

    

* 应答

  | 字段 | data           |
  | ---- | -------------- |
  | cmd  | 0x02           |
  | data | 设备属性值列表 |
  
  * data
  
    ```json
    [
        {
        	"property": 0x25,	// 开关, "on"
        	"value": 0x00		// 0x00: 关， 0x01: 开
    	}
    ]
    ```
  
    

## 5. 设置设备属性

* 请求

  | 字段 | data |
  | ---- | ---- |
  | cmd  | 0x04 |
  | data |      |

  * data

    ```json
    {
        "address": 0x01, 	// 设备地址（可调色温、亮度、可开关的灯）
        "property": 0x25, 	// 设备属性: 亮度
        "value": 0x0A		// 属性值: 亮度开到10%
    }
    ```

* 应答

  | 字段 | data |
  | ---- | ---- |
  | cmd  | 0x03 |
  | data | 无   |



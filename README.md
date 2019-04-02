# gosms
使用go 实现常用的sms服务SDK

目前只实现了腾讯云中指定模板单号码发送功能，后期慢慢实现其他API

* 非官方

### 使用方法
方法很简单，直接上代码

#### 1. 单发短信

```golang
package main

import (
	"fmt"

	"github.com/zboyco/gosms/qcloudsms"
)

func main() {
	// 创建Sender
	sender := &qcloudsms.Sender{
		AppID:  "1234567890",                       // appid
		AppKey: "12345678901234567890123456789000", // appkey
	}

	// 发送短信
	res, err := sender.SingleSend(
		"短信签名", 		 // 短信签名，此处应填写审核通过的签名内容，非签名 ID，如果使用默认签名，该字段填 ""
		86,          		// 国家号
		"13800000000", 		// 手机号
		10000,    			// 短信正文ID
		"123456", 			// 参数1
		"5",      			// 参数2，后面可添加多个参数
	)
	fmt.Println(res, err)
}
```

#### 2. 统一国家码群发短信

所有号码都是同一国家码时可以使用此方法

```golang
package main

import (
	"fmt"

	"github.com/zboyco/gosms/qcloudsms"
)

func main() {
	// 创建Sender
	sender := &qcloudsms.Sender{
		AppID:  "1234567890",                       // appid
		AppKey: "12345678901234567890123456789000", // appkey
	}

	// 统一国家码群发短信
	res, err := sender.MultiSendEachCC(
		"短信签名", // 短信签名，此处应填写审核通过的签名内容，非签名 ID，如果使用默认签名，该字段填 ""
		86, // 国家号
		[]string{
			"13800000000", // 手机号
			"13800000000", // 手机号
			"13800000000", // 手机号
		},
		10000,    // 短信正文ID
		"123456", // 参数1
		"5",      // 参数2，后面可添加多个参数
	)
	fmt.Println(res, err)
}
```

#### 3. 各自国家码群发短信

群发时号码国家码不同时使用此方法

```golang
package main

import (
	"fmt"

	"github.com/zboyco/gosms/qcloudsms"
	"github.com/zboyco/gosms/smsmodels"
)

func main() {
	// 创建Sender
	sender := &qcloudsms.Sender{
		AppID:  "1234567890",                       // appid
		AppKey: "12345678901234567890123456789000", // appkey
	}

	// 各自国家码群发短信
	res, err := sender.MultiSendEachCC(
		"短信签名", // 短信签名，此处应填写审核通过的签名内容，非签名 ID，如果使用默认签名，该字段填 ""
		[]smsmodels.Telphone{
			smsmodels.Telphone{
				Phone: "13800000000", // 手机号
				CC:    86,          // 国家号
			},
			smsmodels.Telphone{
				Phone: "13800000000", // 手机号
				CC:    86,          // 国家号
			},
		},
		10000,    // 短信正文ID
		"123456", // 参数1
		"5",      // 参数2，后面可添加多个参数
	)
	fmt.Println(res, err)
}
```

#### 4. 拉取单个号码短信下发状态

```golang
package main

import (
	"fmt"

	"github.com/zboyco/gosms/qcloudsms"
)

func main() {
	// 创建Sender
	sender := &qcloudsms.Sender{
		AppID:  "1234567890",                       // appid
		AppKey: "12345678901234567890123456789000", // appkey
	}

	// 拉取下发状态
	obj, err := sender.PullSingleStatus(
		86,                    // 国家码
		"13800000000",         // 号码
		"2019-04-01 00:00:00", // 开始日期，注意格式
		"2019-04-03 00:00:00", // 结束日期，注意格式
		100,                   // 拉取最大条数，最大拉取100条
	)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(*obj)
```

#### 5. 拉取短信下发状态

* 此功能需要联系腾讯云开通

```golang
package main

import (
	"fmt"

	"github.com/zboyco/gosms/qcloudsms"
)

func main() {
	// 创建Sender
	sender := &qcloudsms.Sender{
		AppID:  "1234567890",                       // appid
		AppKey: "12345678901234567890123456789000", // appkey
	}

	// 拉取下发状态 此功能需要联系 qcloud sms helper 开通。
	obj, err := sender.PullStatus(100) // 最大拉取100条
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(*obj)
```

#### 6. 拉取单个号码短信回复

```golang
package main

import (
	"fmt"

	"github.com/zboyco/gosms/qcloudsms"
)

func main() {
	// 创建Sender
	sender := &qcloudsms.Sender{
		AppID:  "1234567890",                       // appid
		AppKey: "12345678901234567890123456789000", // appkey
	}

	// 拉取短信回复
	obj, err := sender.PullSingleReply(
		86,                    // 国家码
		"13800000000",         // 号码
		"2019-04-01 00:00:00", // 开始日期，注意格式
		"2019-04-03 00:00:00", // 结束日期，注意格式
		100,                   // 拉取最大条数，最大拉取100条
	)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(*obj)
```

#### 7. 拉取短信回复

* 此功能需要联系腾讯云开通

```golang
package main

import (
	"fmt"

	"github.com/zboyco/gosms/qcloudsms"
)

func main() {
	// 创建Sender
	sender := &qcloudsms.Sender{
		AppID:  "1234567890",                       // appid
		AppKey: "12345678901234567890123456789000", // appkey
	}

	// 拉取短信回复 此功能需要联系 qcloud sms helper 开通。
	obj, err := sender.PullReply(100) // 最大拉取100条
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(*obj)
```
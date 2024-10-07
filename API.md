# 1 用户登录/注册
### [POST] /api/user/login
### 入参
```JSON
{
    "username": "string",   // 用户名，用户用于登录的账户名
    "password": "string"    // 密码，用户的账户密码
}
```
### 出参
```JSON
{
  "code": 200,      // 响应状态码，200表示请求成功
  "message": "Login successful",  // 响应信息，返回登录成功或失败的提示
  "data": {
    "token": "string"     // 鉴权token，登录成功后生成，用于后续接口的鉴权
  }
}
```


# 2 约会计划
## 2.1 【POST】创建&更新约会计划并生成约会方案列表
### /api/plan/schedule/list/generate
### 请求头
```JSON
Authorization: Bearer <token>    // 鉴权令牌，从登录/注册时获取，验证用户身份
```
### 入参
```JSON
{
  "planId": 123, // 约会计划 ID，创建时为空，更新时传值
  "dateInfo": {   // 约会基本信息
    "dateOfWeek": "周一", // 约会日：周一、周二....周日
    "startTime": "2024-09-25 08:00:00",  // 约会开始时间，格式：YYYY-MM-DD HH:mm:ss
    "endTime": "2024-09-25 10:00:00",    // 约会结束时间，格式：YYYY-MM-DD HH:mm:ss
    "foodBudget": "100-300",  // 约会预算 - 餐饮
    "foodSort": [  // 餐厅选择维度，按优先级排序
      "口味", "服务", "环境"
    ],
    "foodPrefer": [  // 饮食偏好，多选
      "江浙", "川渝", "粤菜"
    ],
    "foodType":[ //约会就餐类型偏好：brunch、中晚饭、下午茶、夜宵
      "中晚饭", "夜宵"
    ],
    "activityBudget": "300-500",  // 约会预算 - 活动
    "activityHobbies": [  // 兴趣爱好，多选
      "休闲", "运动", "艺术欣赏"
    ],
    "requires": [  // 约会要求，多选
      "少步行", "无户外"
    ],
    "recentFocus": [ //近期焦虑点/关注点
      "个人成长", "睡眠问题"
    ],
    "trafficPrefer": "开车"  // 交通方式偏好，单选
  },
  "userInfo": [   // 约会双方信息（双向录入）
    {
      "id": "partner_001",   // 约会对象ID
      "departure": "上海市黄浦区", // 出发地
      "geoAddress":["116.481499","39.990475"] // 经纬度
    },
    {
      "id": "partner_001",   // 约会对象ID
      "departure": "上海市黄浦区", // 出发地
      "geoAddress":["116.481499","39.990475"] // 经纬度
    }
  ]
}
```

### 出参
```JSON
{
  "code": 200,  // 响应状态码，200表示请求成功
  "message": "Success",  // 响应信息，成功获取约会日程的提示
  "data": {
    "planId": 123, // 约会计划 ID
    "schedules": [  // 约会方案数组，每个日程包含多个约会事件
      {
        "id": 1,  // 约会方案 ID
        "name": "浪漫晚餐与电影之夜",  // 大模型生成的日程标题
        "pictures": ["", "", "", ""], // 约会方案图片，默认会传入4个
        "outfitRecommend": {  // 约会穿搭建议，适用于整个日程
          "text": "运动",
          "pictures": ["", ""]
        },  
        "itemsNeeded": ["玫瑰花束", "香水", "雨伞"],  // 约会所需的物品，适用于整个日程
        "invitationPr": "想和你共度一个浪漫的晚餐时光，不如一起去体验一下法式美食？",  // 邀约文案
        "topicSuggestions": ["可以聊一下对法式美食的看法，以及最近的生活感悟。"],  // 话题建议
        "tips": ["提前到达，确保有充足的时间浏览展览。"],  // 注意事项（买票）
        "events": [  // 约会事件数组
          {
            "id": 1,  // 约会事件的唯一标识符
            "poiId": 231, // 门店ID
            "name": "浪漫晚餐",  // 约会事件的标题
            "reason": "预定在星级餐厅的浪漫晚餐，享受法式美食。",  // 推荐理由
            "location": "星级餐厅",  // 约会地点
            "geoAddress":["116.481499","39.990475"] // 经纬度
            "phone": "123-456-7890",  // 商家联系电话
            "startTime": "2024-09-25 19:00:00",  // 开始时间，格式：YYYY-MM-DD HH:mm:ss
            "endTime": "2024-09-25 21:00:00",  // 结束时间，格式：YYYY-MM-DD HH:mm:ss
            "status": "0",  // 约会事件是否已完成，0 待开始, 1 进行中, 2 已完成
            "averagePricePerPerson": "500-1000",  // 人均价
            "poiType": "food_dinner" // poi类型
          },
          {
            "id": 2,  // 约会事件的唯一标识符
            "poiId": 232, // 门店ID
            "name": "看电影",  // 约会事件的标题
            "reason": "选择一部轻松的爱情电影，放松心情。",  // 推荐理由
            "location": "CGV影城",  // 约会地点
            "geoAddress":["116.481499","39.990475"] // 经纬度
            "phone": "987-654-3210",  // 商家联系电话
            "startTime": "2024-09-25 21:30:00",  // 开始时间，格式：YYYY-MM-DD HH:mm:ss
            "endTime": "2024-09-25 23:30:00",  // 结束时间，格式：YYYY-MM-DD HH:mm:ss
            "status": "0",  // 约会事件是否已完成，0 待开始, 1 进行中, 2 已完成
            "averagePricePerPerson": "100-300",  // 人均价
            "poiType": "activity" // poi类型
          }
        ]
      }
    ]
  }
}
```

## 2.2 【POST】调整约会方案（支持创建和约会进行中的调整）
### /api/plan/schedule/update
### 请求头
```JSON
Authorization: Bearer <token>    // 鉴权令牌，从登录/注册时获取，验证用户身份
```
### 入参
```JSON
{
  "planId": 123, // 约会计划 ID
  "schedule": {    // 待调整的约会方案
    "id": 1,  // 约会方案 ID
    "name": "浪漫晚餐与电影之夜",  // 大模型生成的日程标题
    "pictures": ["", "", "", ""], // 约会方案图片，默认会传入4个
    "outfitRecommend": {  // 约会穿搭建议，适用于整个日程
      "text": "运动",
      "pictures": ["", ""]
    },
    "itemsNeeded": ["玫瑰花束", "香水", "雨伞"],  // 约会所需的物品，适用于整个日程
    "invitationPr": "想和你共度一个浪漫的晚餐时光，不如一起去体验一下法式美食？",  // 邀约文案
    "topicSuggestions": ["可以聊一下对法式美食的看法，以及最近的生活感悟。"],  // 话题建议
    "tips": ["提前到达，确保有充足的时间浏览展览。"],  // 注意事项（买票）
    "path": "互联宝地T6, 五角场百联又一城, 世纪公园", // 约会路径
    "events": [  // 约会事件数组
      {
        "id": 1,  // 约会事件的唯一标识符
        "poiId": 231, // 门店ID
        "name": "浪漫晚餐",  // 约会事件的标题
        "reason": "预定在星级餐厅的浪漫晚餐，享受法式美食。",  // 推荐理由
        "location": "星级餐厅",  // 约会地点
        "geoAddress":["116.481499","39.990475"] // 经纬度
        "phone": "123-456-7890",  // 商家联系电话
        "startTime": "2024-09-25 19:00:00",  // 开始时间，格式：YYYY-MM-DD HH:mm:ss
        "endTime": "2024-09-25 21:00:00",  // 结束时间，格式：YYYY-MM-DD HH:mm:ss
        "status": "0",  // 约会事件是否已完成，0 待开始, 1 进行中, 2 已完成
        "averagePricePerPerson": "500-1000",  // 人均价
        "poiType": "food_dinner", // poi类型
        "isChange": false, // 是否更换，只有status=0待开始or1进行中，才可以更换
        "changeReson": "" // 更换原因
      },
      {
        "id": 2,  // 约会事件的唯一标识符
        "poiId": 232, // 门店ID
        "reason": "选择一部轻松的爱情电影，放松心情。",  // 推荐理由
        "location": "CGV影城",  // 约会地点
        "geoAddress":["116.481499","39.990475"] // 经纬度
        "phone": "987-654-3210",  // 商家联系电话
        "startTime": "2024-09-25 21:30:00",  // 开始时间，格式：YYYY-MM-DD HH:mm:ss
        "endTime": "2024-09-25 23:30:00",  // 结束时间，格式：YYYY-MM-DD HH:mm:ss
        "status": "0",  // 约会事件是否已完成，0 待开始, 1 进行中, 2 已完成
        "averagePricePerPerson": "100-300",  // 人均价
        "poiType": "activity", // poi类型
        "isChange": true, // 是否更换，只有status=0待开始or1进行中，才可以更换
        "changeReson": "太远了" // 更换原因
      }
    ]
  }
}
```
### 出参
```JSON
{
  "code": 200,  // 响应状态码，200表示请求成功
  "message": "Success",  // 响应信息，成功获取约会日程的提示
  "data": {
    "planId": 123, // 约会计划 ID
    "schedule": {    // 调整后的约会方案
      "id": 1,  // 约会方案 ID
      "name": "浪漫晚餐与电影之夜",  // 大模型生成的日程标题
      "pictures": ["", "", "", ""], // 约会方案图片，默认会传入4个
      "outfitRecommend": {  // 约会穿搭建议，适用于整个日程
        "text": "运动",
        "pictures": ["", ""]
      },
      "itemsNeeded": ["玫瑰花束", "香水", "雨伞"],  // 约会所需的物品，适用于整个日程
      "invitationPr": "想和你共度一个浪漫的晚餐时光，不如一起去体验一下法式美食？",  // 邀约文案
      "topicSuggestions": ["可以聊一下对法式美食的看法，以及最近的生活感悟。"],  // 话题建议
      "tips": ["提前到达，确保有充足的时间浏览展览。"],  // 注意事项（买票）
      "path": "互联宝地T6, 五角场百联又一城, 世纪公园", // 约会路径
      "events": [  // 约会事件数组
        {
          "id": 1,  // 约会事件的唯一标识符
          "poiId": 231, // 门店ID
          "name": "浪漫晚餐",  // 约会事件的标题
          "reason": "预定在星级餐厅的浪漫晚餐，享受法式美食。",  // 推荐理由
          "location": "星级餐厅",  // 约会地点
          "geoAddress":["116.481499","39.990475"] // 经纬度
          "phone": "123-456-7890",  // 商家联系电话
          "startTime": "2024-09-25 19:00:00",  // 开始时间，格式：YYYY-MM-DD HH:mm:ss
          "endTime": "2024-09-25 21:00:00",  // 结束时间，格式：YYYY-MM-DD HH:mm:ss
          "status": "0",  // 约会事件是否已完成，0 待开始, 1 进行中, 2 已完成
          "averagePricePerPerson": "500-1000",  // 人均价
          "poiType": "food_dinner", // poi类型
          "isChange": false // 是否已更换
        },
        {
          "id": 2,  // 约会事件的唯一标识符
          "poiId": 232, // 门店ID
          "reason": "选择一部轻松的爱情电影，放松心情。",  // 推荐理由
          "location": "CGV影城",  // 约会地点
          "geoAddress":["116.481499","39.990475"] // 经纬度
          "phone": "987-654-3210",  // 商家联系电话
          "startTime": "2024-09-25 21:30:00",  // 开始时间，格式：YYYY-MM-DD HH:mm:ss
          "endTime": "2024-09-25 23:30:00",  // 结束时间，格式：YYYY-MM-DD HH:mm:ss
          "status": "0",  // 约会事件是否已完成，0 待开始, 1 进行中, 2 已完成
          "averagePricePerPerson": "100-300",  // 人均价
          "poiType": "activity", // poi类型
          "isChange": true, // 是否已更换
          "changeReson": "变更失败,没有满足要求的日程" // 如果正常变更该字段为空，如果没有找到变更的poi会提示"变更失败,没有满足要求的日程"
        }
      ]
    }
  }
}
```

## 2.3 【POST】确认约会方案
### /api/plan/schedule/ack
### 请求头
```JSON
Authorization: Bearer <token>    // 鉴权令牌，从登录/注册时获取，验证用户身份
```
### 入参
```JSON
{
  "planId": 123, // 约会计划 ID
  "schedule": {    // 调整后的约会方案
    "id": 1,  // 约会方案 ID
    "pictures": ["", "", "", ""], // 约会方案图片，默认会传入4个
    "name": "浪漫晚餐与电影之夜",  // 大模型生成的日程标题
    "outfitRecommend": {  // 约会穿搭建议，适用于整个日程
      "text": "运动",
      "pictures": ["", ""]
    },
    "itemsNeeded": ["玫瑰花束", "香水", "雨伞"],  // 约会所需的物品，适用于整个日程
    "invitationPr": "想和你共度一个浪漫的晚餐时光，不如一起去体验一下法式美食？",  // 邀约文案
    "topicSuggestions": ["可以聊一下对法式美食的看法，以及最近的生活感悟。"],  // 话题建议
    "tips": ["提前到达，确保有充足的时间浏览展览。"],  // 注意事项（买票）
    "events": [  // 约会事件数组
      {
        "id": 1,  // 约会事件的唯一标识符
        "poiId": 231, // 门店ID
        "name": "浪漫晚餐",  // 约会事件的标题
        "reason": "预定在星级餐厅的浪漫晚餐，享受法式美食。",  // 推荐理由
        "location": "星级餐厅",  // 约会地点
        "geoAddress":["116.481499","39.990475"] // 经纬度
        "phone": "123-456-7890",  // 商家联系电话
        "startTime": "2024-09-25 19:00:00",  // 开始时间，格式：YYYY-MM-DD HH:mm:ss
        "endTime": "2024-09-25 21:00:00",  // 结束时间，格式：YYYY-MM-DD HH:mm:ss
        "status": "0",  // 约会事件是否已完成，0 待开始, 1 进行中, 2 已完成
        "averagePricePerPerson": "500-1000",  // 人均价
        "poiType": "food_dinner" // poi类型
      },
      {
        "id": 2,  // 约会事件的唯一标识符
        "poiId": 232, // 门店ID
        "name": "看电影",  // 约会事件的标题
        "reason": "选择一部轻松的爱情电影，放松心情。",  // 推荐理由
        "location": "CGV影城",  // 约会地点
        "geoAddress":["116.481499","39.990475"] // 经纬度
        "phone": "987-654-3210",  // 商家联系电话
        "startTime": "2024-09-25 21:30:00",  // 开始时间，格式：YYYY-MM-DD HH:mm:ss
        "endTime": "2024-09-25 23:30:00",  // 结束时间，格式：YYYY-MM-DD HH:mm:ss
        "status": "0",  // 约会事件是否已完成，0 待开始, 1 进行中, 2 已完成
        "averagePricePerPerson": "100-300",  // 人均价
        "poiType": "activity" // poi类型
      }
    ]
  }
}
```
### 出参
```JSON
{
  "code": 200,
  "message": "Schedule updated successfully",
  "data":true
}
```

## 2.4 【GET】查询所有约会计划
### /api/plan/list
### 请求头
```JSON
Authorization: Bearer <token>    // 鉴权令牌，从登录/注册时获取，验证用户身份
```
### 入参
```JSON
```
### 出参
```JSON
{
  "code": 200,  // 响应状态码，200表示请求成功
  "message": "Success",  // 响应信息，成功获取约会计划列表的提示
  "data": {
    "currentPageNum": 1,
    "totalPageCount": 1,
    "totalCount": 1,
    "pageSize": 10,
    "list": [  // 约会计划数组，每个计划包含约会基本信息和双方信息
      {
        "planId": 123, // 约会计划 ID，创建时为空，更新时传值
        "dateInfo": {   // 约会基本信息
          "dateOfWeek": "周一", // 约会日：周一、周二....周日
          "startTime": "2024-09-25 08:00:00",  // 约会开始时间，格式：YYYY-MM-DD HH:mm:ss
          "endTime": "2024-09-25 10:00:00",    // 约会结束时间，格式：YYYY-MM-DD HH:mm:ss
          "foodBudget": "100-300",  // 约会预算 - 餐饮
          "foodSort": [  // 餐厅选择维度，按优先级排序
            "口味", "服务", "环境"
          ],
          "foodPrefer": [  // 饮食偏好，多选
            "江浙", "川渝", "粤菜"
          ],
          "foodType":[ //约会就餐类型偏好：brunch、中晚饭、下午茶、夜宵
            "中晚饭", "夜宵"
          ],
          "activityBudget": "300-500",  // 约会预算 - 活动
          "activityHobbies": [  // 兴趣爱好，多选
            "休闲", "运动", "艺术欣赏"
          ],
          "requires": [  // 约会要求，多选
            "少步行", "无户外"
          ],
          "recentFocus": [ //近期焦虑点/关注点
            "个人成长", "睡眠问题"
          ],
          "trafficPrefer": "开车"  // 交通方式偏好，单选
        },
        "userInfo": [   // 约会双方信息（双向录入）
          {
            "id": "partner_001",   // 约会对象ID
            "departure": "上海市黄浦区", // 出发地
            "geoAddress":["116.481499","39.990475"] // 经纬度
          },
          {
            "id": "partner_001",   // 约会对象ID
            "departure": "上海市黄浦区", // 出发地
            "geoAddress":["116.481499","39.990475"] // 经纬度
          }
        ],
        "status": 1, // 约会计划状态：0 已创建，1 已采纳方案，2 已复盘
        "schedule": {    // 约会方案，如果 status=0, schedule为空
          "id": 1,  // 约会方案 ID
          "name": "浪漫晚餐与电影之夜",  // 大模型生成的日程标题
          "pictures": ["", "", "", ""], // 约会方案图片，默认会传入4个
          "outfitRecommend": {  // 约会穿搭建议，适用于整个日程
            "text": "运动",
            "pictures": ["", ""]
          },
          "itemsNeeded": ["玫瑰花束", "香水", "雨伞"],  // 约会所需的物品，适用于整个日程
          "invitationPr": "想和你共度一个浪漫的晚餐时光，不如一起去体验一下法式美食？",  // 邀约文案
          "topicSuggestions": ["可以聊一下对法式美食的看法，以及最近的生活感悟。"],  // 话题建议
          "tips": ["提前到达，确保有充足的时间浏览展览。"],  // 注意事项（买票）
          "events": [
              {
              "id": 1,  // 约会事件的唯一标识符
              "poiId": 231, // 门店ID
              "name": "浪漫晚餐",  // 约会事件的标题
              "reason": "预定在星级餐厅的浪漫晚餐，享受法式美食。",  // 推荐理由
              "location": "星级餐厅",  // 约会地点
              "message": "Success",  // 响应信息，成功获取约会日程的提示
              "phone": "123-456-7890",  // 商家联系电话
              "startTime": "2024-09-25 19:00:00",  // 开始时间，格式：YYYY-MM-DD HH:mm:ss
              "endTime": "2024-09-25 21:00:00",  // 结束时间，格式：YYYY-MM-DD HH:mm:ss
              "status": "0",  // 约会事件是否已完成，0 待开始, 1 进行中, 2 已完成
              "averagePricePerPerson": "500-1000", // 人均价
              "poiType": "food_dinner" // poi类型
            },
            {
              "id": 2,  // 约会事件的唯一标识符
              "poiId": 232, // 门店ID
              "name": "看电影",  // 约会事件的标题
              "reason": "选择一部轻松的爱情电影，放松心情。",  // 推荐理由
              "location": "CGV影城",  // 约会地点
              "message": "Success",  // 响应信息，成功获取约会日程的提示
              "phone": "987-654-3210",  // 商家联系电话
              "startTime": "2024-09-25 21:30:00",  // 开始时间，格式：YYYY-MM-DD HH:mm:ss
              "endTime": "2024-09-25 23:30:00",  // 结束时间，格式：YYYY-MM-DD HH:mm:ss
              "status": "0",  // 约会事件是否已完成，0 待开始, 1 进行中, 2 已完成
              "averagePricePerPerson": "100-300",  // 人均价
              "poiType": "activity" // poi类型
            }
          ]
        },
        "review": {
          "userfeedback": "xxxx",  // 用户反馈
          "datefeedback": "xxxx",  // 约会对象反馈
          "tripReview": "今天的约会行程包括：万达影院观看电影，随后在东南亚菜馆用餐，之后前往新日密室逃脱体验，最后在必胜客结束约会。",   // 行程回顾
          "feedbackAnalysis": "希望约会地点更加安静", //反馈分析
          "suggestions": "选择更安静的餐厅，或者考虑户外约会",   // 未来建议
          "conclusion": "这次约会的总体体验很棒，双方均表示期待下一次见面"  // 总结
        }
      }
    ]
  }
}
```

## 2.5 【GET】查询约会计划默认值
### /api/plan/param/default
### 请求头
```JSON
Authorization: Bearer <token>    // 鉴权令牌，从登录/注册时获取，验证用户身份
```
### 入参
```JSON
```
### 出参
```JSON
{
  "code": 200,
  "message": "success",
  "data": {
    "dateInfo": {
      "dateOfWeek": "周一",
      "startTime": "2024-09-30 10:00:00",
      "endTime": "2024-09-30 15:00:00",
      "foodBudget": "100-300",
      "foodSort": [
        "服务",
        "环境",
        "口味"
      ],
      "foodPrefer": [
        "中东菜",
        "北京菜",
        "火锅",
        "日本菜"
      ],
      "foodType": [
        "brunch"
      ],
      "activityBudget": "100-300",
      "activityHobbies": [
        "休闲",
        "养生",
        "自然"
      ],
      "requires": [
        "少步行",
        "少运动"
      ],
      "recentFocus": [
        "个人成长",
        "自然环保'",
        "人际关系"
      ],
      "trafficPrefer": "步行",
      "stage": "情侣",
      "duration": "1-6个月"
    },
    "userInfo": [
      {
        "departure": "娄山关路/威宁路",
        "geoAddress": [
          "121.4052357",
          "31.2104683"
        ]
      },
      {
        "departure": "娄山关路/威宁路",
        "geoAddress": [
          "121.4052357",
          "31.2104683"
        ]
      }
    ]
  }
}
```

# 3 【POST】约会复盘
### /api/review/generate
### 请求头
```JSON
Authorization: Bearer <token>    // 鉴权令牌，从登录/注册时获取，验证用户身份
```
### 入参
```JSON
{
  "planId": 234,   // 约会计划ID，用于标识当前请求的用户
  "userfeedback": "xxxx",  // 用户反馈
  "datefeedback": "xxxx"  // 约会对象反馈
}
```
### 出参
```JSON
{
  "code": 200,   // 响应状态码，200表示请求成功
  "message": "Success",   // 响应信息，成功获取约会复盘数据的提示
  "data": {
    "planId": 234,
    "tripReview": "今天的约会行程包括：万达影院观看电影，随后在东南亚菜馆用餐，之后前往新日密室逃脱体验，最后在必胜客结束约会。",   // 行程回顾
    "feedbackAnalysis": "希望约会地点更加安静", //反馈分析
    "suggestions": "选择更安静的餐厅，或者考虑户外约会",   // 未来建议
    "conclusion": "这次约会的总体体验很棒，双方均表示期待下一次见面"  // 总结
  }
}
```

# 4 我的
## 4.1 登录人资料
### 4.1.1 【GET】查询我的资料
#### /api/user/profile/get
#### 请求头
```JSON
Authorization: Bearer <token>    // 鉴权令牌，从登录/注册时获取，验证用户身份
```
#### 入参
```JSON
```
#### 出参
```JSON
{
  "code": 200,      // 响应状态码，200表示请求成功
  "message": "string",  // 响应信息
  "data": {
    "photo": "string", // 头像
    "username": "string",// 用户名
    "gender": "string",  // "男"或"女"
    "age": "string",     // "18-24", "25-34"等
    "job": "string", // 职业类型
    "mbti": "string",    // MBTI 类型
    "specialty": "string"   // 特长，用英文逗号分隔，示例：手工,艺术鉴赏
  }
}
```
### 4.1.2 【POST】更新我的资料
#### /api/user/profile/update
#### 请求头
```JSON
Authorization: Bearer <token>    // 鉴权令牌，从登录/注册时获取，验证用户身份
```
#### 入参
```JSON
{
   "username": "string",// 用户名
   "gender": "string",  // "男"或"女"
   "age": "string",     // "18-24", "25-34"等
   "job": "string", // 职业类型
   "mbti": "string",    // MBTI 类型
   "specialty": "string"   // 特长，用英文逗号分隔，示例：手工,艺术鉴赏
}
```
#### 出参
```JSON
{
  "code": 200,
  "message": "string"
}
```

## 4.2 约会对象资料
### 4.2.1 【GET】查询约会对象资料
#### 请求头
```JSON
Authorization: Bearer <token>    // 鉴权令牌，从登录/注册时获取，验证用户身份
```
#### /api/date/profile/get
#### 入参
```JSON
```
#### 出参
```JSON
{
  "code": 200,      // 响应状态码，200表示请求成功
  "message": "string",  // 响应信息
  "data": [
    {
      "matchId": "integer",
      "photo": "string", // 头像
      "nickname": "string",// 昵称
      "gender": "string",  // "男"或"女"
      "stage": "string",    // "恋人未满", "情侣", "夫妻"
      "duration": "string", // "<1个月", "1-6个月"等
      "age": "string",      // "18-24", "25-34"等
      "job": "string", // 职业类型
      "mbti": "string",     // MBTI 类型
      "specialty": "string"  // 特长
    }
  ]
}
```

### 4.2.2 【POST】新增&跟新约会对象资料
#### /api/date/profile/update
#### 请求头
```JSON
Authorization: Bearer <token>    // 鉴权令牌，从登录/注册时获取，验证用户身份
```
#### 入参
```JSON
{
  "matchId": 123, 
  "nickname": "string",// 昵称
  "gender": "string",  // "男"或"女"
  "stage": "string",    // "恋人未满", "情侣", "夫妻"
  "duration": "string", // "<1个月", "1-6个月"等
  "age": "string",      // "18-24", "25-34"等
  "job": "string", // 职业类型
  "mbti": "string",     // MBTI 类型
  "specialty": "string"  // 特长
}
```
#### 出参
```JSON
{
  "code": 200,
  "message": "约会对象资料添加成功",
  "matchId": "integer" // 新增或更新的约会对象ID
}
```

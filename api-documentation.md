# Twitter AI自动运营平台 API接口文档

## 1. 用户认证接口

### 1.1 用户登录
- **接口**：`/api/auth/login`
- **方法**：`POST`
- **参数**：
  ```json
  {
    "email": "用户邮箱",
    "password": "密码"
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "登录成功",
    "data": {
      "token": "JWT Token",
      "user": {
        "id": "用户ID",
        "username": "用户名",
        "email": "用户邮箱",
        "avatar": "头像URL",
        "role": "角色"
      }
    }
  }
  ```

### 1.2 用户登出
- **接口**：`/api/auth/logout`
- **方法**：`POST`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "message": "退出成功"
  }
  ```

### 1.3 发送邮箱验证码
- **接口**：`/api/auth/send-verification-code`
- **方法**：`POST`
- **参数**：
  ```json
  {
    "email": "用户邮箱",
    "type": "register/reset" // 注册或重置密码
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "验证码已发送",
    "data": {
      "expireIn": 300 // 验证码有效期(秒)
    }
  }
  ```

### 1.4 验证邮箱验证码
- **接口**：`/api/auth/verify-code`
- **方法**：`POST`
- **参数**：
  ```json
  {
    "email": "用户邮箱",
    "code": "验证码",
    "type": "register/reset" // 注册或重置密码
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "验证成功",
    "data": {
      "verifyToken": "验证令牌" // 用于后续注册或重置密码
    }
  }
  ```

### 1.5 用户注册
- **接口**：`/api/auth/register`
- **方法**：`POST`
- **参数**：
  ```json
  {
    "email": "用户邮箱",
    "password": "密码",
    "verifyToken": "验证令牌" // 邮箱验证成功后获取的令牌
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "注册成功",
    "data": {
      "userId": "用户ID"
    }
  }
  ```

### 1.6 重置密码
- **接口**：`/api/auth/reset-password`
- **方法**：`POST`
- **参数**：
  ```json
  {
    "email": "用户邮箱",
    "password": "新密码",
    "verifyToken": "验证令牌" // 邮箱验证成功后获取的令牌
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "密码重置成功"
  }
  ```

## 2. 主页/分析模块接口

### 2.1 获取概览数据
- **接口**：`/api/analytics/overview`
- **方法**：`GET`
- **参数**：
  ```
  period: 时间段(today, week, month, year)
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "followers": {
        "count": 10500,
        "growth": 3.5,
        "trend": [125, 130, 142, 135, 150, 160, 155]
      },
      "interactions": {
        "count": 2430,
        "growth": 12.2,
        "trend": [230, 250, 220, 270, 300, 290, 310]
      },
      "impressions": {
        "count": 25600,
        "growth": 8.7,
        "trend": [2100, 2300, 2500, 2400, 2700, 2900, 3000]
      },
      "engagement": {
        "rate": 4.2,
        "growth": 1.5,
        "trend": [3.8, 4.0, 4.1, 4.0, 4.2, 4.3, 4.2]
      }
    }
  }
  ```

### 2.2 获取内容效果分析
- **接口**：`/api/analytics/content-performance`
- **方法**：`GET`
- **参数**：
  ```
  limit: 数量(默认10)
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "contents": [
        {
          "id": "推文ID",
          "content": "推文内容",
          "publishTime": "2025-04-12T14:30:00Z",
          "engagementRate": 5.6,
          "likes": 125,
          "retweets": 45,
          "comments": 18,
          "rating": "优秀"
        },
        // 更多内容...
      ]
    }
  }
  ```

### 2.3 获取受众分析
- **接口**：`/api/analytics/audience`
- **方法**：`GET`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "demographics": {
        "gender": [
          {"name": "男性", "value": 65},
          {"name": "女性", "value": 35}
        ],
        "age": [
          {"range": "18-24", "value": 15},
          {"range": "25-34", "value": 40},
          {"range": "35-44", "value": 25},
          {"range": "45+", "value": 20}
        ],
        "location": [
          {"name": "北京", "value": 25},
          {"name": "上海", "value": 20},
          {"name": "广州", "value": 15},
          {"name": "其他", "value": 40}
        ]
      },
      "interests": [
        {"name": "科技", "value": 45},
        {"name": "商业", "value": 35},
        {"name": "创业", "value": 30},
        {"name": "投资", "value": 25},
        {"name": "设计", "value": 20}
      ],
      "activeTime": {
        "dayOfWeek": [15, 18, 22, 20, 25, 30, 28],
        "hourOfDay": [2, 3, 5, 8, 10, 15, 20, 25, 30, 28, 25, 22, 18, 15, 12, 10, 8, 10, 15, 18, 22, 20, 15, 10]
      }
    }
  }
  ```

## 3. AI对话模块接口

### 3.1 发送AI对话消息
- **接口**：`/api/chat/message`
- **方法**：`POST`
- **参数**：
  ```json
  {
    "message": "用户消息内容",
    "conversationId": "会话ID（新会话可为空）"
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "conversationId": "会话ID",
      "message": {
        "id": "消息ID",
        "role": "assistant",
        "content": "AI回复内容",
        "timestamp": "2025-04-20T15:30:45Z"
      }
    }
  }
  ```

### 3.2 获取历史对话
- **接口**：`/api/chat/history`
- **方法**：`GET`
- **参数**：
  ```
  conversationId: 会话ID
  limit: 消息数量(默认20)
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "conversationId": "会话ID",
      "messages": [
        {
          "id": "消息ID",
          "role": "user",
          "content": "用户消息内容",
          "timestamp": "2025-04-20T15:30:00Z"
        },
        {
          "id": "消息ID",
          "role": "assistant",
          "content": "AI回复内容",
          "timestamp": "2025-04-20T15:30:45Z"
        }
        // 更多消息...
      ]
    }
  }
  ```

### 3.3 更新AI偏好设置
- **接口**：`/api/chat/preferences`
- **方法**：`PUT`
- **参数**：
  ```json
  {
    "systemPrompt": "系统提示词内容",
    "tweetStyle": {
      "professional": 4,
      "humorous": 3,
      "detailed": 2
    },
    "topics": ["科技创新", "AI工具", "职场效率", "未来趋势"],
    "continuousLearning": true,
    "memoryRetention": true,
    "responseSpeed": 2
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "偏好设置已更新"
  }
  ```

## 4. 生成推文模块接口

### 4.1 生成推文
- **接口**：`/api/generate/tweet`
- **方法**：`POST`
- **参数**：
  ```json
  {
    "topic": "推文主题",
    "keywords": ["关键词1", "关键词2"],
    "style": "风格(专业/轻松/幽默)",
    "length": "长度(简短/适中/详细)",
    "purpose": "目的(品牌宣传/产品推广/互动话题)",
    "includeHashtags": true,
    "includeEmoji": true,
    "count": 3
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "tweets": [
        {
          "id": "生成ID",
          "content": "推文内容1",
          "hashtags": ["#标签1", "#标签2"],
          "charCount": 120,
          "estimatedEngagement": 4.5
        },
        // 更多推文...
      ]
    }
  }
  ```

### 4.2 获取推荐主题
- **接口**：`/api/generate/recommended-topics`
- **方法**：`GET`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "topics": [
        {
          "id": "主题ID",
          "name": "主题名称",
          "category": "分类",
          "trending": true
        },
        // 更多主题...
      ]
    }
  }
  ```

### 4.3 保存生成的推文
- **接口**：`/api/generate/save`
- **方法**：`POST`
- **参数**：
  ```json
  {
    "tweets": [
      {
        "id": "生成ID",
        "content": "推文内容",
        "hashtags": ["#标签1", "#标签2"],
        "scheduledTime": "2025-04-25T18:30:00Z"
      }
    ]
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "推文已保存",
    "data": {
      "savedIds": ["保存ID1", "保存ID2"]
    }
  }
  ```

## 5. 推文管理模块接口

### 5.1 获取推文列表
- **接口**：`/api/tweets`
- **方法**：`GET`
- **参数**：
  ```
  status: 状态(草稿/已排程/已发布/全部)
  page: 页码
  pageSize: 每页数量
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "tweets": [
        {
          "id": "推文ID",
          "content": "推文内容",
          "hashtags": ["#标签1", "#标签2"],
          "status": "状态",
          "createdAt": "2025-04-12T14:30:00Z",
          "scheduledAt": "2025-04-25T18:30:00Z",
          "publishedAt": "2025-04-25T18:30:00Z",
          "metrics": {
            "likes": 125,
            "retweets": 45,
            "comments": 18,
            "engagement": 5.6
          }
        },
        // 更多推文...
      ],
      "pagination": {
        "total": 42,
        "page": 1,
        "pageSize": 10,
        "pages": 5
      }
    }
  }
  ```

### 5.2 更新推文
- **接口**：`/api/tweets/{id}`
- **方法**：`PUT`
- **参数**：
  ```json
  {
    "content": "更新后的推文内容",
    "hashtags": ["#标签1", "#标签2"],
    "scheduledAt": "2025-04-25T18:30:00Z",
    "status": "草稿/已排程"
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "推文已更新"
  }
  ```

### 5.3 删除推文
- **接口**：`/api/tweets/{id}`
- **方法**：`DELETE`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "message": "推文已删除"
  }
  ```

### 5.4 立即发布推文
- **接口**：`/api/tweets/{id}/publish`
- **方法**：`POST`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "message": "推文已发布",
    "data": {
      "tweetUrl": "Twitter链接URL"
    }
  }
  ```

## 6. 设置模块接口

### 6.1 Twitter账号管理

#### 6.1.1 获取绑定账号
- **接口**：`/api/settings/twitter-accounts`
- **方法**：`GET`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "accounts": [
        {
          "id": "账号ID",
          "username": "@账号名",
          "avatar": "头像URL",
          "followers": 10500,
          "isMain": true,
          "status": "已绑定"
        },
        // 更多账号...
      ]
    }
  }
  ```

#### 6.1.2 添加Twitter账号
- **接口**：`/api/settings/twitter-accounts`
- **方法**：`POST`
- **参数**：
  ```json
  {
    "username": "@账号名",
    "apiKey": "API Key",
    "apiSecret": "API Secret",
    "accessToken": "Access Token",
    "accessSecret": "Access Token Secret"
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "账号添加成功",
    "data": {
      "accountId": "新添加的账号ID"
    }
  }
  ```

#### 6.1.3 设置主账号
- **接口**：`/api/settings/twitter-accounts/{id}/set-main`
- **方法**：`PUT`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "message": "已设置为主账号"
  }
  ```

#### 6.1.4 解绑Twitter账号
- **接口**：`/api/settings/twitter-accounts/{id}`
- **方法**：`DELETE`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "message": "账号已解绑"
  }
  ```

### 6.2 AI大模型API设置

#### 6.2.1 保存API设置
- **接口**：`/api/settings/ai-model`
- **方法**：`POST`
- **参数**：
  ```json
  {
    "provider": "openai/anthropic/deepseek",
    "apiKey": "API密钥",
    "model": "具体模型名称"
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "API设置已保存"
  }
  ```

#### 6.2.2 获取API使用情况
- **接口**：`/api/settings/ai-usage`
- **方法**：`GET`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "openai": {
        "used": 420,
        "total": 1000,
        "percentage": 42,
        "cost": 8.40,
        "budget": 20.00,
        "currency": "USD",
        "resetDate": "2025-05-01"
      },
      "anthropic": {
        "used": 0,
        "total": 1000,
        "percentage": 0,
        "cost": 0.00,
        "budget": 30.00,
        "currency": "USD",
        "resetDate": "2025-05-01"
      },
      "deepseek": {
        "used": 0,
        "total": 1000,
        "percentage": 0,
        "cost": 0.00,
        "budget": 200.00,
        "currency": "CNY",
        "resetDate": "2025-05-01"
      }
    }
  }
  ```

### 6.3 发布策略设置

#### 6.3.1 获取发布策略
- **接口**：`/api/settings/publish-strategy`
- **方法**：`GET`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "autoPublish": true,
      "preferredTimeSlots": ["morning", "evening"],
      "tweetsPerDay": 4,
      "smartInterval": true
    }
  }
  ```

#### 6.3.2 更新发布策略
- **接口**：`/api/settings/publish-strategy`
- **方法**：`PUT`
- **参数**：
  ```json
  {
    "autoPublish": true,
    "preferredTimeSlots": ["morning", "evening"],
    "tweetsPerDay": 4,
    "smartInterval": true
  }
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "message": "发布策略已更新"
  }
  ```

## 7. 通用接口

### 7.1 上传文件/图片
- **接口**：`/api/upload`
- **方法**：`POST`
- **参数**：`multipart/form-data`格式
  ```
  file: 文件数据
  type: 文件类型(image/document)
  ```
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "url": "文件URL",
      "fileName": "文件名",
      "fileSize": 文件大小,
      "fileType": "文件类型"
    }
  }
  ```

### 7.2 系统状态
- **接口**：`/api/system/status`
- **方法**：`GET`
- **参数**：无
- **响应**：
  ```json
  {
    "code": 200,
    "data": {
      "apiStatus": "正常",
      "twitterApiStatus": "正常",
      "lastUpdated": "2025-04-20T15:30:45Z",
      "notifications": [
        {
          "id": "通知ID",
          "type": "info/warning/error",
          "message": "通知内容",
          "timestamp": "2025-04-20T15:30:45Z"
        }
      ]
    }
  }
  ```

## 8. 错误码说明

- `200`: 成功
- `400`: 请求参数错误
- `401`: 未授权/未登录
- `403`: 权限不足
- `404`: 资源不存在
- `409`: 资源冲突
- `429`: 请求频率超限
- `500`: 服务器内部错误

## 9. 接口通用说明

### 9.1 认证方式

所有API请求（除登录接口外）必须包含认证头：

```
Authorization: Bearer {JWT_TOKEN}
```

认证失败将返回401状态码。

### 9.2 请求/响应格式

- 请求内容类型: `application/json`
- 响应内容类型: `application/json`
- 日期时间格式: ISO 8601 (`YYYY-MM-DDThh:mm:ssZ`)

### 9.3 请求限制

- 普通用户: 60次/分钟
- 高级用户: 200次/分钟
- AI对话接口: 20次/分钟

超过限制将返回429状态码。

## 10. WebSocket接口

### 10.1 实时消息通知

- **接口**: `wss://api.domain.com/ws/notifications`
- **认证参数**: `token={JWT_TOKEN}`
- **接收消息格式**:

```json
{
  "type": "notification",
  "data": {
    "id": "通知ID",
    "title": "通知标题",
    "message": "通知内容",
    "level": "info/warning/error",
    "timestamp": "2025-04-20T15:30:45Z",
    "read": false
  }
}
```

### 10.2 AI会话流式响应

- **接口**: `wss://api.domain.com/ws/chat/stream`
- **认证参数**: `token={JWT_TOKEN}&conversationId={CONVERSATION_ID}`
- **发送消息格式**:

```json
{
  "type": "message",
  "data": {
    "message": "用户消息内容"
  }
}
```

- **接收消息格式**:

```json
{
  "type": "token",
  "data": {
    "token": "响应文本片段",
    "messageId": "消息ID",
    "done": false
  }
}
```

响应结束时，会发送`done: true`的消息。

## 11. 批量操作接口

### 11.1 批量删除推文

- **接口**: `/api/tweets/batch-delete`
- **方法**: `POST`
- **参数**:
```json
{
  "ids": ["推文ID1", "推文ID2", "推文ID3"]
}
```
- **响应**:
```json
{
  "code": 200,
  "message": "批量删除成功",
  "data": {
    "successCount": 3,
    "failedIds": []
  }
}
```

### 11.2 批量更新推文状态

- **接口**: `/api/tweets/batch-update`
- **方法**: `PUT`
- **参数**:
```json
{
  "ids": ["推文ID1", "推文ID2"],
  "status": "已排程",
  "scheduledAt": "2025-04-25T18:30:00Z"
}
```
- **响应**:
```json
{
  "code": 200,
  "message": "批量更新成功",
  "data": {
    "successCount": 2,
    "failedIds": []
  }
}
```

## 12. 高级分析接口

### 12.1 竞品分析

- **接口**: `/api/analytics/competitor`
- **方法**: `GET`
- **参数**:
```
competitorHandle: 竞品账号名称(@开头)
metrics: 指标列表(逗号分隔: engagement,growth,topics)
period: 时间段(week, month, quarter)
```
- **响应**:
```json
{
  "code": 200,
  "data": {
    "competitor": {
      "handle": "@competitor",
      "followers": 25600,
      "growth": 5.2,
      "engagement": 3.8,
      "postsPerWeek": 12
    },
    "comparison": {
      "followers": {
        "self": 10500,
        "competitor": 25600,
        "difference": -15100
      },
      "engagement": {
        "self": 4.2,
        "competitor": 3.8,
        "difference": 0.4
      }
    },
    "topPerformingContent": [
      {
        "content": "竞品热门推文内容",
        "likes": 1250,
        "retweets": 450,
        "engagement": 6.8
      }
    ],
    "topics": [
      {"name": "技术革新", "percentage": 35},
      {"name": "产品发布", "percentage": 25},
      {"name": "行业趋势", "percentage": 20}
    ]
  }
}
```

### 12.2 推文最佳发布时间分析

- **接口**: `/api/analytics/optimal-time`
- **方法**: `GET`
- **参数**:
```
dataPoints: 数据点数量(默认30)
```
- **响应**:
```json
{
  "code": 200,
  "data": {
    "weekdays": [
      {"day": "周一", "hour": 19, "score": 85},
      {"day": "周二", "hour": 12, "score": 78},
      {"day": "周三", "hour": 18, "score": 92},
      {"day": "周四", "hour": 20, "score": 88},
      {"day": "周五", "hour": 14, "score": 76},
      {"day": "周六", "hour": 11, "score": 70},
      {"day": "周日", "hour": 16, "score": 65}
    ],
    "heatmap": [
      [10, 15, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 75, 80, 85, 90, 95, 90, 80, 65, 45, 25],
      [15, 20, 25, 35, 40, 45, 50, 60, 65, 70, 75, 80, 75, 70, 65, 70, 80, 85, 80, 75, 70, 60, 40, 30]
      // 更多天...
    ],
    "recommendation": {
      "bestDays": ["周三", "周四", "周一"],
      "bestHours": [19, 20, 18],
      "worstDays": ["周日", "周六"],
      "customSchedule": [
        {"day": "周一", "hours": [12, 19]},
        {"day": "周三", "hours": [18]},
        {"day": "周四", "hours": [20]},
        {"day": "周五", "hours": [14]}
      ]
    }
  }
}
```

## 13. 内容创意API

### 13.1 热门话题推荐

- **接口**: `/api/creative/trending-topics`
- **方法**: `GET`
- **参数**:
```
category: 分类(tech, business, culture等)
limit: 数量(默认10)
```
- **响应**:
```json
{
  "code": 200,
  "data": {
    "topics": [
      {
        "name": "#人工智能",
        "volume": 15600,
        "growth": 23.5,
        "sentiment": "positive",
        "relatedTopics": ["#机器学习", "#AI伦理"]
      },
      // 更多话题...
    ],
    "recommendations": [
      {
        "topic": "#AI创新",
        "angle": "AI在医疗领域的应用前景",
        "sampleTweet": "AI医疗诊断技术正迎来重大突破，可实现95%以上的早期疾病检测准确率。#AI创新 #医疗科技 让我们一起关注这一领域的最新进展！"
      }
    ]
  }
}
```

### 13.2 内容创意生成

- **接口**: `/api/creative/ideas`
- **方法**: `POST`
- **参数**:
```json
{
  "topic": "主题",
  "contentType": "推文/话题串/投票",
  "tone": "专业/幽默/挑战性",
  "goal": "提高互动/增加粉丝/建立品牌",
  "includingElements": ["问题", "数据", "故事", "引用"]
}
```
- **响应**:
```json
{
  "code": 200,
  "data": {
    "ideas": [
      {
        "id": "创意ID",
        "title": "创意标题",
        "description": "详细描述",
        "example": "示例内容",
        "elements": ["问题", "数据"],
        "estimatedEngagement": 4.8
      },
      // 更多创意...
    ]
  }
}
```

## 14. 导入/导出接口

### 14.1 导出分析数据

- **接口**: `/api/export/analytics`
- **方法**: `GET`
- **参数**:
```
format: 格式(csv, xlsx, pdf)
period: 时间段(week, month, quarter, year)
metrics: 指标(全部或特定指标,逗号分隔)
```
- **响应**: 文件下载

### 14.2 导出推文数据

- **接口**: `/api/export/tweets`
- **方法**: `POST`
- **参数**:
```json
{
  "format": "csv/xlsx/pdf",
  "dateRange": {
    "start": "2025-01-01",
    "end": "2025-04-20"
  },
  "status": ["草稿", "已排程", "已发布"],
  "includeMetrics": true
}
```
- **响应**: 文件下载

## 15. API版本控制

当前API版本为v1，所有接口前缀应为`/api/v1/`，例如：
```
/api/v1/tweets
```

版本更新时，新接口将使用新版本号，保证旧接口向后兼容。

## 16. 错误处理示例

错误响应格式：
```json
{
  "code": 400,
  "message": "请求参数错误",
  "errors": [
    {
      "field": "username",
      "message": "用户名不能为空"
    },
    {
      "field": "password",
      "message": "密码长度必须大于6位"
    }
  ]
}
```

---

此API接口文档仅供参考，具体实现可能会有调整。API文档最后更新日期：2025年4月20日。 
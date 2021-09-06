#### 提供标准响应类型

本文档描述了每种 JSON 基本数据类型的可接受值。

### 字符串

* 可接受的值：
  * 字符串
  * `null`

例如：

```javascript
[
  {
    "description": "very descriptive description."
  },
  {
    "description": null
  },
]
```

### 布尔值

* 可接受的值：
  * true
  * false

例如：

```javascript
[
  {
    "provisioned_licenses": true
  },
  {
    "provisioned_licenses": false
  },
]
```

### 数字

* 可接受的值：
  * 数字
  * `null`

注意：一些 JSON 解析器会将精度超过 15 位小数的数字作为字符串返回。
如果您需要大于 15 位小数的精度，请始终为该值返回一个字符串。如果不是，请将这些字符串转换为数字，以便 API 的使用者始终知道期望的值类型。

例如：

```javascript
[
  {
    "average": 27.123
  },
  {
    "average": 12.123456789012
  },
]
```

### 数组

* 可接受的值：
  * 数组

注意：返回空数组而不是NULL当数组中没有值时。

例如：

```javascript
[
  {
    "child_ids": [1, 2, 3, 4],
  },
  {
    "child_ids": [],
  }
]
```

### 对象

* 可接受的值：
  * 对象
  * `null`

例如：

```javascript
[
  {
    "name": "service-production",
    "owner": {
      "id": "5d8201b0..."
     }
  },
  {
    "name": "service-staging",
    "owner": null
  }
]
```

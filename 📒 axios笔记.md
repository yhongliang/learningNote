# 📒 axios POST 请求体传参笔记

## 一、问题背景

在实际开发中，前端调用 POST 接口时，常常遇到：

- 后端要求 **body 是 string**，而实际发送的是 **form-data**
- **Network -> Payload** 显示成 **Form Data** 而非 **Request Payload**
- 请求报错：`The JSON value could not be converted to System.String`

------

## 二、axios 默认行为总结

| 场景                                                         | axios 默认行为                                               | Content-Type                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------------- |
| **data 是对象（object）**                                    | 自动 `JSON.stringify()`                                      | `application/json`                  |
| **data 是字符串（string）且不设置 headers**                  | **不自动加 `Content-Type`**，浏览器 fallback 为 `application/x-www-form-urlencoded` | `application/x-www-form-urlencoded` |
| **data 是字符串，手动设置 `Content-Type: application/json`** | 正常作为 `Request Payload` 发送                              | `application/json`                  |

------

## 三、实际原因分析

- axios 封装时，**如果不手动加 headers**，`data` 是字符串的情况浏览器会 fallback 为 form-data，导致：
  - **请求体以 `Form Data` 形式出现**
  - 后端接收的是字符串但解析错误
- **解决核心：必须手动设置 `Content-Type: application/json`**

------

## 四、最佳实践

### 1️⃣ 推荐请求写法：

```js
axios.post('/api/path', JSON.stringify(body), {
  params: { type: 1 }, // query 参数
  headers: { 'Content-Type': 'application/json' }
});
```

这样：

- Query 参数 -> `params`
- Body -> `Request Payload`（不是 Form Data）
- Content-Type 明确

### 2️⃣ 推荐 axios 封装优化：

在 `request()` 函数中，默认加 Content-Type：

```js
const mergeConfig = {
  baseURL: getOptionsBaseUrl(options.url),
  timeout: this.timeout,
  retry: this.retry,
  withCredentials: this.withCredentials,
  headers: {
    'Content-Type': 'application/json',
    ...(options?.headers || {})
  },
  ...options,
};
```

这样：

- **默认走 JSON**
- **特殊场景可以通过 options 覆盖 Content-Type**

------

## 五、快速检查点

✅ **Network -> Request Headers** 是否有 `Content-Type: application/json`
 ✅ **Network -> Payload** 是否为 **Request Payload** 而不是 **Form Data**
 ✅ **后端接口**如果是 `[FromBody]string` 必须 `body` 是 **纯字符串** 不是 key-value 结构

------

## 六、总结

- **对象类型**：axios 自动序列化
- **字符串类型**：必须显式 `Content-Type: application/json`，避免 fallback
- **封装 axios** 时统一加默认 `Content-Type`，可选手动覆盖更安全
- **Debug** 看 **Network tab** → Request Payload vs Form Data 最直观

------

🟣 **核心记忆一句话**：

> axios 只对对象自动处理，string 请求体必须手动指定 `Content-Type`，否则浏览器可能默认走 form-data！
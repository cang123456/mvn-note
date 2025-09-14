axios 是一个基于 Promise 的 HTTP 客户端，主要用于浏览器和 Node.js 环境中发送 HTTP 请求。它支持多种请求方法（如 GET、POST 等），能拦截请求和响应，转换请求 / 响应数据，且使用简单直观，是前端开发中常用的网络请求工具。

### 一、安装 axios

根据项目环境选择安装方式：

**npm 或 yarn 安装**（适用于 Vue、React 等工程化项目）：

```
npm install axios  # npm 安装
# 或
yarn add axios     # yarn 安装
```

**CDN 引入**（适用于简单 HTML 页面）：

```
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```

### 二、基本使用

axios 的核心是通过调用方法发送请求，最常用的是 `GET`（获取数据）和 `POST`（提交数据）。

#### 1. 发送 GET 请求

用于从服务器获取数据，基本语法：

```javascript
// 方式1：使用 .then() 处理成功/失败
axios.get('目标URL', { params: { /* URL参数 */ } })
  .then(response => {
    // 请求成功时执行（response 是响应对象）
    console.log('请求成功：', response.data); // response.data 是服务器返回的核心数据
  })
  .catch(error => {
    // 请求失败时执行（如网络错误、404等）
    console.log('请求失败：', error);
  });

// 方式2：使用 async/await（更简洁，需在 async 函数中使用）
async function fetchData() {
  try {
    const response = await axios.get('https://api.example.com/data', {
      params: { id: 1, type: 'info' } // URL参数会自动拼接到URL后：?id=1&type=info
    });
    console.log('数据：', response.data);
  } catch (error) {
    console.log('出错了：', error);
  }
}
fetchData();
```

#### 2. 发送 POST 请求

用于向服务器提交数据（如表单提交、创建资源），基本语法：

```javascript
// 方式1：.then() 形式
axios.post('目标URL', { /* 要提交的数据 */ })
  .then(response => {
    console.log('提交成功：', response.data);
  })
  .catch(error => {
    console.log('提交失败：', error);
  });

// 方式2：async/await 形式
async function submitData() {
  try {
    const response = await axios.post('https://api.example.com/submit', {
      username: '张三',
      age: 20,
      hobby: ['篮球', '游戏']
    });
    console.log('服务器返回：', response.data);
  } catch (error) {
    console.log('提交失败：', error);
  }
}
submitData();
```

### 三、核心配置

可以通过配置项自定义请求（如设置请求头、超时时间等），支持全局配置或单次请求配置。

#### 1. 单次请求配置

在发送请求时通过参数指定配置：

```javascript
axios.get('https://api.example.com/data', {
  params: { id: 1 }, // URL参数
  headers: { 'Content-Type': 'application/json' }, // 请求头
  timeout: 5000, // 超时时间（毫秒），超过则取消请求
  responseType: 'json' // 预期响应数据类型（json/blob/text等）
})
.then(response => { /* ... */ });
```

#### 2. 全局配置

设置全局默认值，所有请求都会生效：

```javascript
// 设置全局基础URL（后续请求可省略域名部分）
axios.defaults.baseURL = 'https://api.example.com';
// 设置全局超时时间
axios.defaults.timeout = 10000;
// 设置全局请求头
axios.defaults.headers.common['Authorization'] = 'Bearer token123'; // 例如携带身份令牌
```

### 四、响应结构

axios 的响应对象包含以下核心属性（处理响应时常用）：

```javascript
axios.get('...').then(response => {
  response.data; // 服务器返回的核心数据（最常用）
  response.status; // HTTP状态码（如200表示成功，404表示未找到）
  response.statusText; // HTTP状态描述（如"OK"）
  response.headers; // 响应头信息
  response.config; // 请求时的配置信息
});
```

### 五、错误处理

请求失败时（网络错误、4xx/5xx 状态码），可以在 `catch` 中处理：

```javascript
axios.get('...').catch(error => {
  if (error.response) {
    // 服务器有响应（如404、500）
    console.log('状态码：', error.response.status);
    console.log('响应数据：', error.response.data);
  } else if (error.request) {
    // 无响应（如网络中断）
    console.log('无响应：', error.request);
  } else {
    // 其他错误（如请求配置错误）
    console.log('错误信息：', error.message);
  }
});
```

### 六、拦截器

axios 支持拦截请求和响应，用于统一处理（如添加令牌、处理错误）：

```javascript
// 请求拦截器（发送请求前执行）
axios.interceptors.request.use(
  config => {
    // 例如：给所有请求添加令牌
    config.headers.Authorization = 'Bearer token123';
    return config; // 必须返回配置，否则请求会中断
  },
  error => {
    // 请求配置错误时执行
    return Promise.reject(error);
  }
);

// 响应拦截器（收到响应后执行）
axios.interceptors.response.use(
  response => {
    // 只返回核心数据，简化后续处理
    return response.data;
  },
  error => {
    // 统一处理错误（如令牌过期跳转登录）
    if (error.response?.status === 401) {
      window.location.href = '/login'; // 跳转到登录页
    }
    return Promise.reject(error);
  }
);
```

### 总结

axios 是前端网络请求的强大工具，核心特点：



- 支持 Promise API，可配合 `async/await` 简化代码
- 支持多种请求方法（GET/POST/PUT/DELETE 等）
- 可拦截请求和响应，实现统一处理
- 自动转换 JSON 数据，无需手动解析



入门阶段重点掌握 GET/POST 请求的基本用法，以及如何处理成功 / 失败的情况，后续可深入学习拦截器、实例化配置等高级功能。

![【哲风壁纸】02插画-Twotwo](axios入门.assets\【哲风壁纸】02插画-Twotwo.png)






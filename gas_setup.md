# Google Apps Script 配置指南

## 第一步：创建 Google 表格

1. 打开 Google Drive → 新建 → Google 表格
2. 表格命名为「恩博补助金留资」
3. 第一行填写表头（按顺序）：
   `时间戳 | 公司名称 | 姓名 | 电话 | 微信号 | 身份 | 地区 | 员工数 | 需求 | 营业额 | 匹配项目`

## 第二步：创建 Apps Script

1. 在表格中点击「扩展程序」→「Apps Script」
2. 删除默认代码，粘贴以下代码：

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  let data;
  try { data = JSON.parse(e.postData.contents); } catch(err) { data = {}; }

  sheet.appendRow([
    data.timestamp || new Date().toLocaleString('zh-CN'),
    data.company  || '',
    data.name     || '',
    data.phone    || '',
    data.wechat   || '',
    data.q1       || '',
    data.q3       || '',
    data.q4       || '',
    data.q7       || '',
    data.q8       || '',
    data.matches  || '',
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({status:'ok'}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

## 第三步：部署为 Web App

1. 点击右上角「部署」→「新建部署」
2. 类型选「网页应用」
3. 执行身份：「我」
4. 谁可以访问：「所有人」
5. 点击「部署」→ 复制生成的 URL

## 第四步：填入 HTML

打开 index.html，找到这一行：
```
const GAS_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL';
```
把 `YOUR_GOOGLE_APPS_SCRIPT_URL` 替换为第三步复制的 URL，然后告诉小青蛙推送更新。

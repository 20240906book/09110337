答案
使用Node.js 和Express 框架設計註冊路由並處理表單提交的基本步驟如下：
1. 環境設置
首先，確保你已經安裝了Node.js 和Express。可以使用以下指令來建立一個新的Node.js 專案並安裝Express：
bash
mkdir myapp
cd myapp
npm init -y
npm install express body-parser mongoose

2. 設置Express 應用
在項目根目錄下建立一個文件，並設定基本的Express 應用：app.js
javascript
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');

const app = express();
const PORT = process.env.PORT || 3000;

// 連接到 MongoDB 數據庫
mongoose.connect('mongodb://localhost:27017/myapp', { useNewUrlParser: true, useUnifiedTopology: true });

// 設置 body-parser 中介軟體
app.use(bodyParser.urlencoded({ extended: true }));

// 設置靜態文件夾
app.use(express.static('public'));

// 啟動伺服器
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});

3. 創建用戶模型
在項目中創建一個文件夾，並在其中創建一個文件，用於定義用戶模型：modelsUser.js
javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
    username: { type: String, required: true },
    password: { type: String, required: true }
});

module.exports = mongoose.model('User', userSchema);

4. 設定註冊路由
在中新增註冊路由，處理表單提交：app.js
javascript
const User = require('./models/User');

// 渲染註冊頁面
app.get('/register', (req, res) => {
    res.sendFile(__dirname + '/public/register.html'); // 假設有一個 register.html 文件
});

// 處理註冊表單提交
app.post('/register', async (req, res) => {
    const { username, password } = req.body;

    // 創建新用戶
    const newUser = new User({ username, password });
    
    try {
        await newUser.save();
        res.send('註冊成功！');
    } catch (error) {
        res.status(400).send('註冊失敗：' + error.message);
    }
});

5. 建立註冊頁面
在文件夾中建立一個文件，包含註冊表單：publicregister.html
xml
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>註冊</title>
</head>
<body>
    <h1>註冊</h1>
    <form action="/register" method="POST">
        <label for="username">用戶名：</label>
        <input type="text" id="username" name="username" required>
        <br>
        <label for="password">密碼：</label>
        <input type="password" id="password" name="password" required>
        <br>
        <button type="submit">註冊</button>
    </form>
</body>
</html>

6. 啟動服務器
在終端中執行以下命令啟動伺服器：
bash
node app.js

然後在瀏覽器中訪問來查看註冊頁面。http://localhost:3000/register
總結
這是一個基本的Node.js 和Express 註冊系統的實現範例，涵蓋了用戶模型的創建、註冊路由的設置和表單提交的處理。根據需求，可以進一步擴展功能，例如新增密碼加密、驗證用戶輸入等。

https://20240906book.github.io/09110337/

drag-n-drop
/style.css

要有複製練習的能力一分鐘 50個
產品 流程觀看 最多最常出現的

表達 制式化 固定 寒士庫 

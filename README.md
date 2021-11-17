# vue-login-exercise

## Live Demo
https://ruoshiuan.github.io/vuejs-login-exercise/

## About
#### 介紹實作的功能：
1. 兩個登入按鈕：一個打成功 (/login/ok) 的API，一個打失敗 (/login/error) 的API
2. 焦點離開 input 欄位時，觸發帳號或密碼的格式驗證，若有錯誤會顯示紅字訊息
3. 按下登入按鈕，需符合驗證格式才發送API Request
4. 登入失敗會清除密碼欄位，顯示"登入失敗"
5. 登入成功會隱藏登入表單，顯示使用者名稱的訊息，並將 token 存入 localStorage
6. 重新造訪網頁，以 POST 請求方式把存在 localStorage 中的 token 放在 Body 裡發送API Request(假設有伺服器和資料庫幫忙做登入狀態驗證)
7. 登出會清除 localStorage 儲存的 token

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

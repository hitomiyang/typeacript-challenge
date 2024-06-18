# JavaScript HTTP 客戶端庫

## 作用

-   發送請求：這些庫可以發送各種 HTTP 請求（GET、POST、PUT、DELETE 等）。
-   處理回應：接收伺服器的回應，並對回應資料進行處理，例如解析 JSON。
-   錯誤處理：處理 HTTP 錯誤，如 404 或 500 狀態碼。
-   攔截請求和回應：在請求發送前或回應接收後進行攔截，進行額外的操作（如添加標頭、處理錯誤等）。
-   跨域請求：處理跨域問題，確保在不同域名間進行安全的資料交換。

## 常見的 JavaScript HTTP 客戶端庫

1. Axios:

    - Axios 是一個基於 Promise 的 HTTP 客戶端，用於瀏覽器和 Node.js。
    - 特點：支持請求和回應攔截器、轉換請求和回應資料、自動轉換 JSON 資料、取消請求、攜帶 Cookie、客製化錯誤處理等。
    - [官方網站](https://axios-http.com/)

    ```javascript
    import axios from 'axios';

    axios
        .get('/user?ID=12345')
        .then(response => {
            console.log(response.data);
        })
        .catch(error => {
            console.log(error);
        });
    ```

2. Fetch API:

    - Fetch API 是現代瀏覽器內建的，用於發送 HTTP 請求。
    - 特點：基於 Promise，支持進行 Fetch 請求、處理流、跨域請求、在全域範圍內可用等。
    - [Fetch API MDN 文檔](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

    ```javascript
    fetch('/user?ID=12345')
    then(response => response.json())
    then(data => console.log(data))
    catch(error => console.log(error));
    ```

3. jQuery AJAX:

    - jQuery 是一個廣泛使用的 JavaScript 庫，提供簡便的 AJAX 方法。。
    - 特點：支持簡單的 AJAX 請求、回調函數、全域 AJAX 事件處理等。

    ```javascript
    $.ajax({
        url: '/user',
        data: { ID: 12345 },
        success: function (response) {
            console.log(response);
        },
        error: function (error) {
            console.log(error);
        },
    });
    ```

4. SuperAgent:

    - SuperAgent 是一個靈活且直觀的 HTTP 客戶端，適用於 Node.js 和瀏覽器。
    - 特點：支持鏈式調用、攔截請求、流式文件上傳和下載等。

    ```javascript
    import superagent from 'superagent';

    superagent
        .get('/user')
        .query({ ID: 12345 })
        .then(response => {
            console.log(response.body);
        })
        .catch(error => {
            console.log(error);
        });
    ```


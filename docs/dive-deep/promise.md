# Promise

-   `Promise` 是 JavaScript 的一個內建對象，用於表示一個異步操作的最終完成（或失敗），以及其結果值。
-   `Promise` 在 Typescript 中是一個更完整的實現，包括` then`、`catch` 和 `finally` 方法。
-   `Promise` 的實現遵循了一個稱為 "Promise/A+" 的規範，這是一個社群制定的標準，用於確保不同實現之間的互操作性。

## 在 Javascript 中，`Promise` 的基本用法如下：

```javascript
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('成功');
    }, 1000);
});

promise
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error(error);
    })
    .finally(() => {
        console.log('完成');
    });
```

??? info "延伸閱讀"

    - `catch` 方法：
        - 用於註冊一個回調，用來處理 Promise 被拒絕的情況。
        - `catch` 是 `then` 方法的一個語法糖，等價於 `.then(null, onRejected)`。
    - `finally` 方法：
        - 用於註冊一個回調，無論 `Promise` 最終是完成還是被拒絕，都會執行這個回調。
        - `finally` 不接受任何參數，且回調不會改變 `Promise` 的最終值或理由。

## 在 TypeScript 中，`Promise` 的定義如下：

```typescript
interface Promise<T> {
    then<TResult1 = T, TResult2 = never>(
        onfulfilled?: ((value: T) => TResult1 | PromiseLike<TResult1>) | undefined | null,
        onrejected?: ((reason: any) => TResult2 | PromiseLike<TResult2>) | undefined | null,
    ): Promise<TResult1 | TResult2>;

    catch<TResult = never>(
        onrejected?: ((reason: any) => TResult | PromiseLike<TResult>) | undefined | null,
    ): Promise<T | TResult>;

    finally(onfinally?: (() => void) | undefined | null): Promise<T>;
}
```

從定義中可以看到，`Promise` 比 `PromiseLike` 擁有更多的方法，包括 `catch` 和 `finally`，這些方法提供了更完整的異步操作管理。

---

# PromiseLike

-   是 TypeScript 中的一個內建接口，用來描述任何具有 `then` 方法的物件，但不需要完全符合 Promise/A+ 規範。
-   `PromiseLike` 只要求實現 `then` 方法，而不要求其他 `Promise` 規範中的行為。
-   `PromiseLike` 更加通用，可以用來描述任何類似 `Promise` 的物件，而不僅僅是完全符合 Promise/A+ 規範的 `Promise`。

## `PromiseLike` 的定義：

```typescript
interface PromiseLike<T> {
    then<TResult1 = T, TResult2 = never>(
        onfulfilled?: ((value: T) => TResult1 | PromiseLike<TResult1>) | undefined | null,
        onrejected?: ((reason: any) => TResult2 | PromiseLike<TResult2>) | undefined | null,
    ): PromiseLike<TResult1 | TResult2>;
}
```

這個定義表示任何具有 `then` 方法的物件都可以被認為是 `PromiseLike`。這使得我們的型別更具通用性，可以處理所有符合這個結構的異步操作。

---

# Promise/A+ 規範

1. “promise” 是一個符合此標準且包含 then 方法的 function 或 object。
2. “thenable” 是一個包含 then 方法的 function 或 object。
3. “value” 是一個 JavaScript 中合法的數值，包括 undefined 、thenable 或 promise。
4. “exception” 是一個使用 throw 拋出的錯誤值。
5. “reason” 是一個說明 promise 為何被拒絕 (reject) 的原因。

## Promise/A+ 核心概念內容

-   狀態（States）：

    -   `Promise` 必須處於以下三種狀態之一：

        1. 等待中（pending）
        2. 已完成（fulfilled）
        3. 已拒絕（rejected）。

    -   初始狀態為等待中，並且可以轉變為已完成或已拒絕，但不能從已完成或已拒絕狀態轉回等待中狀態。

-   值或理由（Value or Reason）：

    一個已完成的 `Promise` 必須有一個值（value），一個已拒絕的 Promise 必須有一個拒絕理由（reason）。這些值或理由是不可變的。

-   `then` 方法：

    -   `Promise` 必須實現一個 `then` 方法來訪問最終值或拒絕原因。
    -   `then` 方法接受兩個參數：`onFulfilled` 和 `onRejected`，它們分別是 `Promise` 完成或拒絕時的回調函數。

-   處理程序（Handlers）：

    -   `then` 方法可以被同一個 `Promise` 多次調用，每次調用都會返回一個新的 `Promise`。
    -   每次 `then` 方法調用中的回調函數必須按照調用順序異步執行。

-   `then` 方法的返回值：

    -   `then` 方法必須返回一個新的 `Promise`。
    -   如果 `onFulfilled` 或 `onRejected` 返回一個值 `x`，則運行 `Promise` 解決過程，將 `x` 的值傳遞給新的 `Promise`。
    -   如果 `onFulfilled` 或 `onRejected` 拋出一個異常，則新的 `Promise` 必須以該異常為拒絕原因。

---

# 差異比較

|              | Promise                                                | PromiseLike                                                                      |
| ------------ | ------------------------------------------------------ | -------------------------------------------------------------------------------- |
| **方法數量** | `then`,`catch`, `finally`                              | `then`                                                                           |
| **應用範圍** | 是標準的 Javascript 對象，具有完整的異步操作管理功能。 | 更加通用，可以用來表示任何具有 `then` 方法的物件，無論他是否是標準的 `Promise`。 |
| **詳細說明** | 用於具體的異步操作，提供了完整的異步操作處理能力。     | 主要用於型別定義中，允許接受類似 `Promise` 的物件。                              |

# 總結

-   `PromiseLike` 是 TypeScript 中用來表示任何具有 `then` 方法的物件的接口，而 `Promise` 則是 JavaScript 的標準對象，提供了更完整的異步操作管理功能。

-   `PromiseLike` 只要求實現 `then` 方法，而不要求實現 `catch` 和 `finally` 方法，也不需要完全符合 Promise/A+ 規範。

-   使用 `PromiseLike` 更加通用，可以用來描述任何類似 `Promise` 的物件，而不僅僅是完全符合 Promise/A+ 規範的 `Promise`。


# PromiseLike

-   是一個內建的接口，用來描述任何具有 `then` 方法的物件。
-   這個接口允許我們在 TypeScript 中處理所有類似 `Promise` 的物件，而不僅僅是標準的 `Promise`。

`PromiseLike` 的定義：

```typescript
interface PromiseLike<T> {
    then<TResult1 = T, TResult2 = never>(
        onfulfilled?: ((value: T) => TResult1 | PromiseLike<TResult1>) | undefined | null,
        onrejected?: ((reason: any) => TResult2 | PromiseLike<TResult2>) | undefined | null,
    ): PromiseLike<TResult1 | TResult2>;
}
```

這個定義表示任何具有 `then` 方法的物件都可以被認為是 `PromiseLike`。這使得我們的型別更具通用性，可以處理所有符合這個結構的異步操作。

# Promise

-   `Promise` 是 JavaScript 的一個內建對象，用於表示一個異步操作的最終完成（或失敗），以及其結果值。
-   `Promise` 在 Typescript 中是一個更完整的實現，包括` then`、`catch` 和 `finally` 方法。

在 TypeScript 中，`Promise` 的定義如下：

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

# 差異比較

|                 | PromiseLike                                                                      | Promise                                                |
| --------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------ |
| **方法數量**    | `then`                                                                           | `then`,`catch`, `finally`                              |
| **應用範圍**    | 更加通用，可以用來表示任何具有 `then` 方法的物件，無論他是否是標準的 `Promise`。 | 是標準的 Javascript 對象，具有完整的異步操作管理功能。 |
| **Description** | 主要用於型別定義中，允許接受類似 `Promise` 的物件。                              | 用於具體的異步操作，提供了完整的異步操作處理能力。     |

# 總結

`PromiseLike` 是 TypeScript 中用來表示任何具有 `then` 方法的物件的接口，而 `Promise` 則是 JavaScript 的標準對象，提供了更完整的異步操作管理功能。

使用 `PromiseLike` 可以讓我們的型別更具通用性，適用於更廣泛的異步操作場景。


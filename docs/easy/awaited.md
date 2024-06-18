# Challenge 00189

| Property         | Description                                                                                                                                                                      |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Utility Type** | `Awaited<T>`                                                                                                                                                                     |
| **Release**      | 4.5                                                                                                                                                                              |
| **Description**  | This type is meant to model operations like `await` in `async` functions, or the `.then()` method on `Promise`s - specifically, the way that they recursively unwrap `Promise`s. |

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/00189-easy-awaited/README.md) | [Take the Challenge](https://tsch.js.org/189/play)

## 題目

If we have a type which is a wrapped type like Promise, how we can get the `type` which is inside the wrapped type?

For example: if we have `Promise<ExampleType>` how to get ExampleType?

這題目要求我們實現一個泛型 `MyAwaited<T>`，這個泛型會從一個類似於 `Promise` 這樣的包裝型別中提取出內部的型別。

## 範例

```typescript
type ExampleType = Promise<string>;

type Result = MyAwaited<ExampleType>; // 預期結果是 string
```

在這個例子中，我們希望 `MyAwaited<ExampleType>` 返回 `string`，即提取出 `Promise<string>` 中的 `string` 型別。

---

## 會用到的技術

在這題中，我們會用到 TypeScript 的以下幾個特性：

1. 泛型（Generics）：

    - 泛型允許我們定義在多個型別上都能運行的函式或類別。
    - 在這題中，我們會用 `T` 作為泛型參數，代表我們要處理的包裝型別。

2. 條件型別（Conditional Types）：

    - 條件型別允許我們根據條件來選擇型別。
    - 在這題中，我們會用條件型別來檢查型別是否為 `Promise`。

3. 型別推斷（Type Inference）：
    - TypeScript 可以自動推斷某些表達式的型別。
    - 在這題中，我們會用型別推斷來提取 `Promise` 中的內部型別。

## 解答

```typescript
// 處理 Promise 及其內含的嵌套 .then()
type MyAwaited<T> = T extends PromiseLike<infer U> ? MyAwaited<U> : T;
```

在這段代碼中：

1. 泛型定義：`type MyAwaited<T>`：這裡定義了一個泛型 `MyAwaited`，它接受一個參數 `T`，這個 `T` 可以是任何型別。

2. 條件型別：`T extends PromiseLike<infer U> ? MyAwaited<U> : T`：這是一個條件型別，用來檢查 `T` 是否符合某種特定的型別模式。如果條件成立，則返回 `MyAwaited<U>`；否則，返回 `T`。

3. 檢查 `T` 是否為 `PromiseLike`：

    - 檢查(`T extends PromiseLike<infer U>`)：

        - 這個條件型別用來檢查 `T` 是否可以被賦值給 `PromiseLike<U>`。
        - `PromiseLike<any>` 是 TypeScript 中的一個內建型別，它代表了具有類似 `Promise` 行為的物件，也就是說，這個物件具有 `then` 方法。使用 `PromiseLike<any>` 可以讓我們更通用地處理 `Promise` 及其類似的物件。
        - `PromiseLike` 接受一個型別參數 `U`，這裡的 `U` 是一個待推斷的型別變數，表示 `then` 方法所返回的型別。

    - 型別推斷(`infer U`)：

        - 如果 `T` 可以被賦值給 `PromiseLike<U>`，則 `infer U` 會啟動型別推斷機制，推斷出 `U` 的具體型別。
        - 這意味著 `U` 是 `PromiseLike<T>` 的內部型別。

4. 遞歸應用 `MyAwaited`：

    - 如果 `T` 符合 `PromiseLike<U>` 的模式，我們遞歸地應用 `MyAwaited<U>`。這樣可以處理嵌套的 `Promise`。
    - 舉例：`Promise<Promise<string>>`
        - 第一次能推斷出 `U` 為 `Promise<string>`。
        - 第二次能推斷出 `U` 為 `string`。

5. 返回 T 本身：
    - 如果 `T` 不符合 `PromiseLike<U>` 的模式，則條件型別返回 `T` 本身。
    - 這意味著 `T` 不是一個 `Promise` 或類似 `Promise` 的物件，直接返回其型別。

這樣，我們就完成了一個可以從 `PromiseLike` 中提取內部型別的泛型型別 `MyAwaited<T>`。

---

#### 補充說明

`T` 是 `Promise<string>`

```typescript
type MyAwaited<T> = T extends PromiseLike<infer U> ? MyAwaited<U> : T;

type ExampleType = Promise<string>;
type Result = MyAwaited<ExampleType>; // 預期結果是 string
```

1. `ExampleType` 是 `Promise<string>`。
2. 檢查 `ExampleType extends PromiseLike<infer U>` 是否成立。
    - 是的，`ExampleType` 可以賦值給 `PromiseLike<U>`，其中 `U` 被推斷為 `string`。
3. 因為條件成立，結果型別是 `MyAwaited<U>`，即 `MyAwaited<string>`。
4. 因為 `string` 不符合 `PromiseLike<U>` 的模式，直接返回 `string`。

---

#### 補充說明 2

`T` 是 `Promise<Promise<string>>`

```typescript
type MyAwaited<T> = T extends PromiseLike<infer U> ? MyAwaited<U> : T;

type ExampleType = Promise<Promise<string>>;
type Result = MyAwaited<ExampleType>; // 預期結果是 string
```

1. `ExampleType` 是 `Promise<Promise<string>>`。
2. 檢查 `ExampleType extends PromiseLike<infer U>` 是否成立。
    - 是的，`ExampleType` 可以賦值給 `PromiseLike<U>`，其中 `U` 被推斷為 `Promise<string>`。
3. 因為條件成立，結果型別是 `MyAwaited<U>`，即 `MyAwaited<Promise<string>>`。
4. 再次應用 `MyAwaited`，檢查 `Promise<string> extends PromiseLike<infer V>` 是否成立。
    - 是的，`Promise<string>` 可以賦值給 `PromiseLike<V>`，其中 `V` 被推斷為 `string`。
5. 因為條件成立，結果型別是 `MyAwaited<V>`，即 `MyAwaited<string>`。
6. 因為 `string` 不符合 `PromiseLike<U>` 的模式，直接返回 `string`。

這樣，我們就確保了 `MyAwaited` 泛型可以提取出 `Promise` 中的型別，並且在 `T` 不是 `Promise` 的情況下返回 `T` 本身的原始型別。


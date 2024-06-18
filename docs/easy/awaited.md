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

`PromiseLike<any>` 是 TypeScript 中的一個內建型別，它代表了具有類似 `Promise` 行為的物件，也就是說，這個物件具有 `then` 方法。使用 `PromiseLike<any>` 可以讓我們更通用地處理 `Promise` 及其類似的物件。

1. `T extends PromiseLike<infer U>`：

    - 檢查：

        - 這個條件型別用來檢查 `T` 是否可以被賦值給 `PromiseLike<U>`。
        - 這裡的 `PromiseLike` 是 TypeScript 內建的泛型型別，代表具有 `then` 方法的物件。
        - 這裡的 `U` 是一個待推斷的型別變數。

    - 型別推斷：

        - 如果 `T` 可以被賦值給 `PromiseLike<U>`，則 `infer U` 會啟動型別推斷機制，推斷出 `U` 的具體型別。
        - 這意味著 `U` 是 `PromiseLike<T>` 的內部型別。
        - 如果 `T` 是 `PromiseLike`，則推斷出內部型別 `U`，並遞歸應用 `MyAwaited`。

這樣，我們就完成了一個可以從 `Promise` 中提取內部型別的泛型型別 `MyAwaited<T>`。

---

#### 補充說明

`T` 是 `Promise<string>`

```typescript
const T = Promise<string>;
type MyAwaited<T> = T extends Promise<infer U> ? U : T;

type ExampleType = Promise<string>;
type Result = MyAwaited<ExampleType>; // 預期結果是 string
```

1. 檢查：`ExampleType` 是 `Promise<string>`，這符合 `Promise<U>` 的模式。
2. 推斷：`infer U` 會推斷出 `U` 為 `string`。
3. 返回：因為條件成立，返回 `U`，即 `string`。

---

#### 補充說明 2

`T` 是 `string`

```typescript
const T = string;
type MyAwaited<T> = T extends Promise<infer U> ? U : T;

type ExampleType = string;
type Result = MyAwaited<ExampleType>; // 預期結果是 string
```

1. 檢查：`ExampleType` 是 `string`，不符合 `Promise<U>` 的模式。
2. 推斷：因為 `string` 不能賦值給 `Promise<U>`，所以不進行型別推斷。
3. 返回：因為條件不成立，返回 `T` 本身，即 `string`。

這樣，我們就確保了 `MyAwaited` 泛型可以提取出 `Promise` 中的型別，並且在 `T` 不是 `Promise` 的情況下返回 `T` 本身的原始型別。

---

## 解答 2

```typescript

```

```typescript
type A = Awaited<Promise<string>>;
//   type A = string

type B = Awaited<Promise<Promise<number>>>;
//   type B = number

type C = Awaited<boolean | Promise<number>>;
//   type C = number | boolean

type D = Awaited<boolean | Promise<Promise<string>>>;
//   type D = string | boolean
```

可以返回 `Promise` 內的 `Promise` 的 `type` 類型。


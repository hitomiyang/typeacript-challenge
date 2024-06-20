# Challenge 00268

`If<C, T, F>`

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/00268-easy-if/README.md) | [Take the Challenge](https://tsch.js.org/268/play)

## 題目

Implement the util type `If<C, T, F>` which accepts condition `C`, a truthy value `T`, and a falsy value `F`. `C` is expected to be either true or false while `T` and `F` can be any type.

這道題目要求我們實現一個名為 `If<C, T, F>` 的工具型別。這個工具型別接受一個條件 `C`，以及在條件為真時返回的型別 `T` 和在條件為假時返回的型別 `F`。其中，條件 `C` 預期是 `true` 或 `false`，而 `T` 和 `F` 可以是任何型別。

## 範例

```typescript
type A = If<true, 'a', 'b'>; // 預期結果是 'a'
type B = If<false, 'a', 'b'>; // 預期結果是 'b'
```

簡單來說，`If<C, T, F>` 就像是 TypeScript 裡的三元運算子 `C ? T : F`，根據條件 `C` 的真偽來決定回傳的類型是 `T` 還是 `F`。

也可以說是用來根據 `C` 的值來返回 `T` 或 `F` 的型別。

---

## 會用到的技術

在這題中，我們會用到 TypeScript 的以下特性：

1. 條件型別（Conditional Types）：

    - 條件型別允許我們根據條件來選擇型別。
    - 條件型別的基本形式是 `T extends U ? X : Y`，這表示如果型別 `T` 可以賦值給型別 `U`，那麼結果型別是 `X`，否則是 `Y`。

2. 布林值 (Boolean Values)：理解布林值在條件型別中的應用。

3. 類型推論 (Type Inference)：TypeScript 如何根據給定的條件來推斷結果型別。

## 解答

```typescript
type If<C extends boolean, T, F> = C extends true ? T : F;
```

在這段代碼中：

1. `C extends boolean`：用 `extends` 約束 `C` 必須是布林值（`true` 或 `false`）。

2. `C extends true ? T : F`：這表示如果 `C` 是 `true`，那麼結果型別是 `T`，否則是 `F`。

這樣，我們就完成了 `If` 工具類型的實作，並且可以根據條件來靈活地選擇型別。


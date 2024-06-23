# Challenge 00898

`Includes`

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/00898-easy-includes/README.md) | [Take the Challenge](https://tsch.js.org/898/play)

## 題目

Implement the JavaScript `Array.includes` function in the type system. A type takes the two arguments. The output should be a boolean `true` or `false`.

這道題目要求我們在型別系統中實現 JavaScript 的 `Array.includes` 函數。這個型別需要接受兩個參數，並且輸出一個布林值 `true` 或 `false`，以表示第一個參數的陣列是否包含第二個參數的值。

## 範例

```typescript
type isPillarMen = Includes<['Kars', 'Esidisi', 'Wamuu', 'Santana'], 'Dio'>; // 預期結果是 `false`
```

---

## 會用到的技術

1. 泛型（Generics）：

    - 泛型允許我們定義在多個型別上都能運行的函式或類別。
    - 在這裡，我們需要泛型來處理不同的陣列型別。

2. 條件型別（Conditional Types）：

    - 條件型別允許我們根據條件來選擇型別。

3. 遞歸型別（Recursive Types）：

    - 遞歸類型允許我們在型別中使用自身，適合用來處理嵌套結構。

## 解答

```typescript
type Includes<T extends readonly any[], U> = T extends [infer First, ...infer Rest]
    ? Equal<First, U> extends true
        ? true
        : Includes<Rest, U>
    : false;
```

在這段代碼中：

1. `T extends [infer First, ...infer Rest] ?`：

    - 我們檢查陣列 `T` 是否可以被分解成第一個元素 `First` 和剩餘元素 `Rest`。
    - 這裡就算 `T` 陣列中只剩下一個元素，也可以被順利的分解，只是第二個 `Rest` 元素會是一個空陣列。

2. `Equal<First, U> extends true`：

    - 比較第一個元素，如果 `T` 可以順利被分解，我們就利用 `Equal` 去檢查 `First` 是否等於 `U`。

3. `? true`：

    - 如果`First` 等於 `U` 的話，返回 `true`。

4. `: Includes<Rest, U>`：

    - 如果 `First` 不等於 `U` 的話，在這裡遞規檢查 `Includes`。
    - 重新把 `Rest` 塞進 `T` 中，重複前面的檢查。

5. `: false`：如果 `T` 不是一個能被分解成 `First` 及 `Rest` 的陣列（不是一個有元素的陣列）的話，返回 `false`。

這樣，我們就展示了 `Includes` 工具型別如何在型別系統中實現 JavaScript 的 `Array.includes` 函數。


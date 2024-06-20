# Challenge 00533

`Concat`

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/00533-easy-concat/README.md) | [Take the Challenge](https://tsch.js.org/533/play)

## 題目

Implement the JavaScript `Array.concat` function in the type system. `A` type takes the two arguments. The output should be a new array that includes inputs in ltr order

這題要求我們在 TypeScript 的型別系統中實作 JavaScript 的 `Array.concat` 函數。`Array.concat` 函數會接收兩個陣列作為參數，然後返回一個包含兩個輸入陣列元素的新陣列，元素順序保持從左到右。

## 範例

```typescript
type Result = Concat<[1], [2]>; // 預期結果是 [1, 2]
```

---

## 會用到的技術

在這題中，我們會用到 TypeScript 的以下特性：

1.  陣列型別（Array Types）：我們需要處理的是陣列型別，並且合併這些陣列。
2.  泛型（Generics）：泛型讓我們可以寫出適用於多個型別的代碼。在這裡，我們需要泛型來處理不同的陣列型別。
3.  展開運算符（Spread Operator）：在型別系統中，我們可以使用類似於展開運算符的方式來合併陣列。

## 解答

```typescript
type Concat<T extends readonly any[], U extends readonly any[]> = [...T, ...U];
```

在這段代碼中：

1. `T extends readonly any[]`：這表示 `T` 必須是任意型別的唯讀陣列。
2. `U extends readonly any[]`：這表示 `U` 也必須是任意型別的唯讀陣列。
3. `[...T, ...U]`：這使用了展開運算符來合併兩個陣列。

這樣，我們就實現了一個可以合併兩個陣列的工具型別 `Concat`。


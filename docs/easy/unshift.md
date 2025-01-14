# Challenge 03060

`Unshift`

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/03060-easy-unshift/README.md) | [Take the Challenge](https://tsch.js.org/3060/play)

## 題目

Implement the generic version of `Array.unshift`

這道題目要求我們在型別系統中實現 JavaScript 的 `Array.unshift` 函數。這個型別需要接受兩個參數，第一個參數是陣列，第二個參數是要被添加到陣列開頭的元素。最終結果應該是一個新陣列，包含新添加的元素以及原始陣列的所有元素。

## 範例

```typescript
type Result = Unshift<[1, 2], 0>; // 預期結果是 [0, 1, 2]
```

---

## 會用到的技術

1. 泛型（Generics）：泛型讓我們可以針對不同的型別進行操作。
2. 元組型別（Tuple Types）：我們需要處理的是元組型別，並將新元素添加到元組開頭。
3. 展開運算符（Spread Operator）：在型別系統中，我們可以使用展開運算符來合併元組。

## 解答

```typescript
type Unshift<T extends any[], U> = [U, ...T];
```

在這段代碼中：

我們使用了 TypeScript 的展開運算符來將原始陣列 `T` 的所有元素展開，並在末尾添加新元素 `U`。

1. `type Unshift<T extends any[], U>`：定義一個泛型型別 `Unshift`，它接受兩個參數 `T` 和 `U`。
2. `T extends any[]`：這表示 `T` 必須是一個陣列型別。
3. `[U, ...T]`：這是陣列展開語法，表示將元素 `U` 放在陣列 `T` 的開頭。

這樣，我們就實現了一個可以將新元素添加到陣列末尾的工具型別 `Unshift`。


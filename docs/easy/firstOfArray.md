# Challenge 00014

`First<T>`

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/00014-easy-first/README.md) | [Take the Challenge](https://tsch.js.org/14/play)

## 題目

Implement a generic `First<T>` that takes an Array `T` and returns its first element's type.

這題目要求我們實現一個泛型 `First<T>`，這個泛型接受一個陣列 `T`，並返回其第一個元素的型別。

## 範例

```typescript
type arr1 = ['a', 'b', 'c'];
type arr2 = [3, 2, 1];

type head1 = First<arr1>; // 預期結果是 'a'
type head2 = First<arr2>; // 預期結果是 3
```

---

## 會用到的技術

在這題中，我們會用到 TypeScript 的以下幾個特性：

1. 泛型（Generics）：

    - 泛型允許我們定義在多個型別上都能運行的函式或類別。
    - 在這題中，我們會用 `T` 作為泛型參數，代表我們要處理的陣列

2. 條件型別（Conditional Types）：

    - 條件型別允許我們根據條件來選擇型別。
    - 在這題中，我們會用條件型別來檢查陣列是否為空。

3. 型別推斷（Type Inference）：
    - TypeScript 可以自動推斷某些表達式的型別。
    - 在這題中，我們會用型別推斷來獲取陣列的第一個元素的型別。

## 解答

```typescript
type First<T extends any[]> = T extends [infer F, ...any[]] ? F : never;
```

在這段代碼中：

1. `T extends any[]`：這個約束條件表示 `T` 必須是陣列。
2. `T extends [infer F, ...any[]]`：這是一個條件型別：

    - 用來檢查 `T` 是否可以分解為一個以 `F` 為第一個元素的陣列。
    - 這意味著 `T` 必須是至少有一個元素的陣列。

3. `infer F` ：這是 TypeScript 的型別推斷語法，推斷並賦值：

    - 用來推斷陣列 `T` 的第一個元素的型別，並將其賦值給 `F`。
    - 如果 `T` 符合 `[infer F, ...any[]]`，則 `infer F` 推斷出 `T` 的第一個元素的型別，並將這個型別賦值給 `F`。

4. `? F : never`：如果 `T` 可以分解為一個以 `F` 為第一個元素的陣列，則返回 `F`；否則返回 `never`。

---

## 解答 2

```typescript
type First<T extends any[]> = T extends [] ? never : T[0];
```

在這段代碼中：

1. `T extends any[]`：這個約束條件表示 `T` 必須是陣列。
2. `T extends []`：檢查 `T` 是否是一個空的 array。
3. `? never : T[0]`：如果 `T` 是一個空的 array 則不成立，返回 `never`，否則取 `T` 內的第一個物件的 type。

這樣，我們就完成了一個可以返回陣列第一個元素型別的泛型型別 `First<T>`。

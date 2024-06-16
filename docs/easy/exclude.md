# Challenge 00043

`Exclude<T, U>`

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/00043-easy-exclude/README.md) | [Take the Challenge](https://tsch.js.org/43/play)

## 題目

Implement the built-in `Exclude<T, U>`

`Exclude from T those types that are assignable to U`

這題目要求我們實現內建的 `Exclude<T, U>`，這個泛型的作用是從型別 `T` 中排除那些可以賦值給型別 `U` 的型別。

## 範例

```typescript
type Result = MyExclude<'a' | 'b' | 'c', 'a'>; // 預期結果是 'b' | 'c'
```

---

## 會用到的技術

在這題中，我們會用到 TypeScript 的以下幾個特性：

1. 泛型（Generics）：

    - 泛型允許我們定義在多個型別上都能運行的函式或類別。
    - 在這題中，我們會用 `T` 和 `U` 作為泛型參數，分別代表我們要處理的型別和要排除的型別。

2. 條件型別（Conditional Types）：

    - 條件型別允許我們根據條件來選擇型別。
    - 在這題中，我們會用條件型別來檢查哪些型別需要被排除。

## 解答

```typescript
type MyExclude<T, U> = T extends U ? never : T;
```

在這段代碼中：

1. `T extends U`：

    - 我們使用 `extends` 來檢查 `T` 是否可以賦值給 `U`，如果 `T` 中的某個成員可以賦值給 `U`，條件為真。

2. `? never : T`：這是條件型別的語法。
    - 如果條件為真（即 `T` 中的某個成員可以賦值給 `U` ），則返回 `never`。
    - 如果條件為假（即 `T` 中的某個成員不能賦值給 `U` ），則返回 `T` 中的該成員。

這樣，我們就完成了一個與內建 `Exclude<T, U>` 相同功能的泛型型別。

---

### 補充說明

假設我們有以下型別：

```typescript
type Result = MyExclude<'a' | 'b' | 'c', 'a'>; // 預期結果是 'b' | 'c'
```

在這裡：

-   `T` 是 `'a' | 'b' | 'c'`：這是一個聯合型別。
-   `U` 是 `'a'`：這是一個單一型別。

TypeScript 會對 T 中的每個成員進行檢查：

-   當 `T` 為 `'a'` 時，`'a' extends 'a'` 為真，結果為 `never`。
-   當 `T` 為 `'b'` 時，`'b' extends 'a'` 為假，結果為 `'b'`。
-   當 `T` 為 `'c'` 時，`'c' extends 'a'` 為假，結果為 `'c'`。

最終結果是將所有的結果組合起來，因此：

```typescript
type Result = MyExclude<'a' | 'b' | 'c', 'a'>;
// 結果為 'b' | 'c'
```

這意味著我們從 `T` 中排除了可以賦值給 `U` 的那些成員。


# Challenge 00007

`Readonly<Type>`

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/00007-easy-readonly/README.md) | [Take the Challenge](https://tsch.js.org/7/play)

## 題目

Implement the built-in `Readonly<T>` generic without using it.

Constructs a type with all properties of T set to readonly, meaning the properties of the constructed type cannot be reassigned.

這題的要求是讓我們實現內建的 `Readonly<T>` 泛型，但不能直接使用內建的 `Readonly<T>`。

這個泛型會構造一個所有屬性都設定為唯讀（readonly）的型別，意思是這個型別的屬性不能被重新賦值。

## 範例

```typescript
// 我們有一個 Todo 的介面：
interface Todo {
    title: string;
    description: string;
}

// 接著，我們用我們實現的 MyReadonly<Todo> 來創建一個 todo 物件：
const todo: MyReadonly<Todo> = {
    title: 'Hey',
    description: 'foobar',
};

//然後，如果我們嘗試重新賦值 todo 的屬性，就會產生錯誤：
todo.title = 'Hello'; // Error: cannot reassign a readonly property
todo.description = 'barFoo'; // Error: cannot reassign a readonly property
```

---

## 會用到的技術

在這題中，我們會用到 TypeScript 的以下幾個特性：

1. 泛型（Generics）：

    - 泛型允許我們定義在多個型別上都能運行的函式或類別。
    - 在這裡，T 是一個泛型參數，表示我們要處理的型別。

2. 映射型別（Mapped Types）：
    - 映射型別讓我們可以創建一個基於另一個型別的新型別，並對其進行某種變換。
    - 映射型別使用 in 關鍵字來迭代型別的屬性。

## 解答

```typescript
type MyReadonly<T> = {
    readonly [P in keyof T]: T[P];
};
```

在這段代碼中：

1. keyof T 會產生 T 的所有屬性名稱的聯合型別（子集合）。
    - keyof 是用來取得某個型別的所有屬性鍵，並返回這些鍵組成的聯合型別。
      例如，對於 `interface Todo { title: string; description: string; }`，`keyof Todo` 會產生聯合型別 `"title" | "description"`。
2. `[P in keyof T]` 是在迭代 T 的所有屬性。
    - in 關鍵字用來遍歷某個型別的所有屬性。
      在這個例子中，我們使用 `[P in keyof T]` 來遍歷型別 T 的所有屬性，P 代表每一個屬性鍵。
3. `readonly` 關鍵字將每個屬性設為唯讀。
4. `T[P]` 表示每個屬性的型別。

這樣，我們就完成了一個與內建 `Readonly<T>` 相同功能的型別。


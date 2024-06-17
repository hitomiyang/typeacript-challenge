# Challenge 00004

| Property         | Description                                                                                                         |
| ---------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Utility Type** | `Pick<T, K>`                                                                                                        |
| **Release**      | 2.1                                                                                                                 |
| **Description**  | Constructs a type by picking the set of properties `Keys` (string literal or union of string literals) from `Type`. |

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/00004-easy-pick/README.md) | [Take the Challenge](https://tsch.js.org/4/play)

## 題目

Implement the built-in `Pick<T, K>` generic without using it.

Constructs a `type` by picking the set of properties `K` from `T`

這題目要求我們實現內建的 `Pick<T, K>` 泛型，但不能直接使用內建的 `Pick<T, K>`。這個泛型會從型別 `T` 中挑選出屬性集合 `K`，構造出一個新的型別。

## 範例

```typescript
// 我們有一個 Todo 的介面：
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

// 接著，我們用我們實現的 MyPick<Todo, 'title' | 'completed'> 來創建一個新的型別 TodoPreview：
type TodoPreview = MyPick<Todo, 'title' | 'completed'>;

const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
};
```

這個範例中，TodoPreview 型別只包含 Todo 型別中的 title 和 completed 屬性。

---

## 會用到的技術

1. 泛型（Generics）：

    - 泛型允許我們定義在多個型別上都能運行的函式或類別。
    - 在這題中，我們會用 `T` 和 `K` 作為泛型參數，分別代表我們要處理的型別和要挑選的屬性鍵。

2. 映射型別（Mapped Types）：
    - 這是 TypeScript 的一個強大特性，可以用來創建一個基於另一個型別的新型別，並對其進行變換。
    - 在這題中，我們會用映射型別來構造新型別。

## 解答

```typescript
type MyPick<T, K extends keyof T> = {
    [P in K]: T[P];
};
```

在這段代碼中：

1. 我們創建一個新的 `Type` 叫做 `MyPick`，這是一個泛型型別，接收兩個參數：`T` 和 `K`。

2. `K extends keyof T` 是我們約束 `K` 必須是 `T` 的屬性鍵的子集合。

3. 在這個例子中，可以得出 `K = {'title' | 'completed'}`

4. `{[P in K]: T[P];}` 這段可以理解為：在新型別中，`[P in K]` 是屬性鍵，而 `T[P]` 則是這個屬性鍵的型別。

5. `[P in K]` 這部份表示我們用 `P` 去遍歷 `K`，`P` 是一個臨時變數，依次取出 `K` 中的值。

6. `T[P]` 代表 `T` 中屬性 `P` 的型別：

    - 當 P 等於 'title' 時，`T['title']` 是 string。
    - 當 P 等於 'completed' 時，`T['completed']` 是 boolean。

7. 這個新的型別包含 `K` 中的屬性鍵以及它們在 `T` 中對應的型別。

8. 所以，這個新的型別 MyPick`<Todo, 'title' | 'completed'>` 將會是：

    ```javascript
    {
        'title': string,
        'completed': boolean
    }
    ```

這樣，我們就完成了一個與內建 `Pick<T, K>` 相同功能的泛型型別。


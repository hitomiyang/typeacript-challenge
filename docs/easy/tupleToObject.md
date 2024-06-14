# Challenge 00011

`TupleToObject<T>`

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/00011-easy-tuple-to-object/README.md) | [Take the Challenge](https://tsch.js.org/11/play)

## 題目

Given an array, transform it into an object type and the key/value must be in the provided array.

這題目要求我們將一個陣列轉換為一個物件型別，並且物件的鍵和值必須是來自這個陣列中的元素。

## 範例

```typescript
// 我們有一個陣列：
const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const;

// 我們期望將這個陣列轉換為如下所示的物件型別：
type result = TupleToObject<typeof tuple>;
// expected { 'tesla': 'tesla', 'model 3': 'model 3', 'model X': 'model X', 'model Y': 'model Y'}
```

---

## 會用到的技術

在這題中，我們會用到 TypeScript 的以下幾個特性：

1. 字面量類型推斷（Literal Type Inference）：

    - 我們使用 as const 來讓 TypeScript 推斷出一個陣列的字面量類型。

2. 泛型（Generics）：

    - 泛型允許我們定義在多個型別上都能運行的函式或類別。
    - 在這裡，我們會用 T 作為泛型參數，代表我們要處理的陣列。

3. 映射型別（Mapped Types）：
    - 這是 TypeScript 的一個強大特性，可以用來創建一個基於另一個型別的新型別，並對其進行變換。
    - 在這題中，我們會用映射型別來構造新型別。

## 解答

```typescript
type TupleToObject<T extends readonly any[]> = {
    [K in T[number]]: K;
};
```

在這段代碼中：

我們使用 `readonly any[]` 作為泛型 T 的約束條件。

1. `T extends readonly any[]`：這表示 T 必須是一個只讀陣列。

2. `T[number]`：這會取得陣列 T 中所有元素的聯合類型。
    - 比如，對於 `['tesla', 'model 3', 'model X', 'model Y']`，`T[number]` 會是 `'tesla' | 'model 3' | 'model X' | 'model Y'`。
3. `[K in T[number]]`：這表示我們遍歷 T 中的每個元素 K，並將其作為新物件的鍵和值。

在這個範例中，`TupleToObject<typeof tuple>` 會將 tuple 中的每個元素轉換為物件的鍵和值。

這樣，我們就完成了一個可以將元組轉換為物件型別的泛型型別 `TupleToObject<T>`。

---

#### 補充說明

```typescript
const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const;
```

這裡的 as const 是關鍵所在。as const 讓 TypeScript 將 tuple 推斷為一個只讀的元組，且其元素的類型是字面量類型。

-   沒有 as const 的情況

    如果我們不使用 as const，則 tuple 的類型會被推斷為 `string[]`，這意味著所有元素都會被認為是 string 類型，而不是具體的字面量類型。

    ```typescript
    const tuple = ['tesla', 'model 3', 'model X', 'model Y'];
    // 這裡 tuple 的類型會是 string[]
    ```

-   使用 as const 的情況

    使用 as const 之後，tuple 的類型會被推斷為以下形式：

    ```typescript
    const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const;
    // 這裡 tuple 的類型會是 readonly ['tesla', 'model 3', 'model X', 'model Y']
    ```

    這樣，tuple 中的每個元素都會被視為具體的字面量類型 `'tesla' | 'model 3' | 'model X' | 'model Y'`。

-   實現 TupleToObject 的原因

    正因為有了字面量類型推斷，我們才能將這些具體的字面量類型作為鍵和值來構建新型別。


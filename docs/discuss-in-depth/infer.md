-   在 TypeScript 中，`infer` 是一個關鍵字，用於在條件型別（Conditional Types）中引入型別推斷（Type Inference）。
-   `infer` 允許我們在條件型別的 `true` 分支中推斷出某個型別，並將其賦值給一個型別變數。這在處理複雜的型別轉換和提取時特別有用。
-   當我們使用 `infer` 時，它會在條件型別中推斷出一個型別並賦值給一個變數。這個過程同時進行，類似於**邊宣告邊賦值**。

# 運作順序

1. 條件檢查：

    - 首先，條件型別會檢查左側的型別是否符合右側的模式。
    - 如果符合，則條件成立；否則條件不成立。

2. 型別推斷：

    - 如果條件成立，`infer` 關鍵字會啟動型別推斷機制，推斷出特定的型別並將其賦值給指定的變數。

3. 返回型別：

    - 根據條件的成立與否，條件型別會返回不同的結果。
    - 當條件成立時，返回推斷出的型別；當條件不成立時，返回另一個指定的型別（通常是 `never`）。

---

# 基本用法：

```typescript
type GetReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
```

這段代碼定義了一個泛型型別 `GetReturnType`，用於提取函數的返回值型別。

-   條件型別：

    `T extends (...args: any[]) => infer R`：檢查 `T` 是否是一個函數。
    `infer R`：如果 `T` 是函數型別，則推斷出返回值型別並賦值給 `R`。

-   返回型別：

    如果 `T` 是函數，則返回 `R`（函數的返回值型別）。
    否則，返回 `never`。

## 範例

```typescript
type Example = (a: number, b: string) => boolean;
type Result = GetReturnType<Example>; // 結果是 boolean
```

在這個例子中，`GetReturnType<Example>` 將推斷出 `boolean`，因為 `Example` 函數的返回值型別是 `boolean`。

---

# 其他用法：

`infer` 可以用於提取各種型別的信息，例如數組元素型別、元組元素型別等。

## 範例

```typescript
type ElementType<T> = T extends (infer U)[] ? U : never;
```

這段代碼用於提取數組的元素型別。

-   條件型別：

    `T extends (infer U)[]`：檢查 `T` 是否是一個數組。
    `infer U`：如果 `T` 是數組型別，則推斷出元素型別並賦值給 `U`。

-   返回型別：

    如果 `T` 是數組，則返回 `U`（數組的元素型別）。
    否則，返回 `never`。

## 提取元組第一個元素型別

```typescript
type FirstElement<T> = T extends [infer U, ...any[]] ? U : never;
```

這段代碼用於提取元組的第一個元素型別。

-   條件型別：

    `T extends [infer U, ...any[]]`：檢查 `T` 是否是一個元組，且有至少一個元素。
    `infer U`：如果 `T` 是元組，則推斷出第一個元素型別並賦值給 `U`。

-   返回型別：

    如果 `T` 是元組，則返回 `U`（元組的第一個元素型別）。
    否則，返回 `never`。

---

# 與其他方法的比較

`infer` 與其他 TypeScript 中的型別操作方法可能會讓人混淆，特別是與 `keyof` 和 `extends` 等關鍵字。這裡簡單比較一下它們的用途和區別：

-   `infer`：用於在條件型別中進行型別推斷，提取和操作型別中的具體部分。
-   `keyof`：用於獲取某個物件型別的所有鍵，返回這些鍵組成的聯合型別。
-   `extends`：用於約束泛型型別或在條件型別中進行型別檢查。


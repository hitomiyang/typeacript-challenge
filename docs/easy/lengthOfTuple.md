# Challenge 00018

## Length`<T>`

[Link](https://github.com/type-challenges/type-challenges/blob/main/questions/00018-easy-tuple-length/README.md) | [Take the Challenge](https://tsch.js.org/18/play)

## 題目

For given a tuple, you need create a generic Length, pick the length of the tuple.

這題目要求我們實現一個泛型 Length<T>，這個泛型會接收一個 tuple，並返回這個 tuple 的長度。

## 範例:

```typescript
// 我們有以下兩個tuple：
type tesla = ['tesla', 'model 3', 'model X', 'model Y'];
type spaceX = ['FALCON 9', 'FALCON HEAVY', 'DRAGON', 'STARSHIP', 'HUMAN SPACEFLIGHT'];

// 使用我們實現的 Length<T> 泛型，應該得到以下結果：
type teslaLength = Length<tesla>; // expected 4
type spaceXLength = Length<spaceX>; // expected 5
```

---

## 會用到的技術

在這題中，我們會用到 TypeScript 的以下幾個特性：

1. 泛型（Generics）：

    - 泛型允許我們定義在多個型別上都能運行的函式或類別。
    - 在這裡，T 是一個泛型參數，表示我們要處理的元組型別。

2. 元組（Tuples）：

    - 元組是一種特殊的陣列，長度固定且每個元素的型別可以不同。
    - 在這題中，我們的泛型 Length 會針對 tuple 進行操作。

## 解答

```typescript
type Length<T extends readonly any[]> = T['length'];
```

在這段代碼中：

我們使用 readonly any[] 作為泛型 T 的約束條件。

readonly any[] 允許 T 是只讀的元組，也允許 T 是可變的陣列或元組。這是因為只讀的約束條件涵蓋了可變陣列和元組的所有行為和屬性。

不過，這樣的約束條件並不能嚴格區分 T 是元組還是一般的陣列。這是因為在 TypeScript 的型別系統中，元組和陣列之間的區別主要在於它們的使用方式和定義方式，而不是在於它們的型別本身。在實際應用中，這樣的約束條件通常已經足夠滿足需求，因為我們大多數情況下只需要確保 T 是一個只讀的陣列或元組。

另外，我們利用 T['length'] 來取得元組的長度。這是一個索引查詢，查詢型別 T 的 length 屬性，返回元組 T 的長度。

1. T extends readonly any[]：

    - 我們使用 extends 關鍵字來約束泛型。

    - 使用 readonly 的原因：這讓泛型可以接受

        1. 只讀的元組，例如 readonly [string, number]
        2. 可變的陣列，例如 [string, number]
        3. 可變的元組，例如 [string, number]

    - 如果拿掉 readonly，這將讓泛型 T 只能接受

        1. 可變的陣列
        2. 可變的元組

    - 使用 any[] 或 使用 unknown[] 的差異：
        1. any[]：我們明確表示我們不關心陣列中的元素類型，任何類型都可以。
        2. unknown[]：我們表示我們不確定陣列中的元素類型，但我們希望保留型別檢查的安全性。

    在這個情境下，由於我們只需要取得陣列的長度，不涉及對元素的操作，因此 readonly any[] 和 readonly unknown[] 都是安全的選擇。

2. T['length']：我們利用 TypeScript 的型別系統來取得元組 T 的長度。
    - 這是一個索引查詢，查詢型別 T 的 length 屬性，返回 元組 T 的長度。

這樣，我們就完成了一個可以返回任意元組長度的泛型型別 `Length<T>`。


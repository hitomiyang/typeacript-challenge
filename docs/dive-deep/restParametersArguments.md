在 TypeScript 中，`Rest Parameters`（剩餘參數）和 `Arguments`（參數）是一組用來處理不定數量參數的方法。這在處理函數調用時非常有用，特別是當我們不知道會有多少個參數傳入時。

# Rest Parameters（剩餘參數）

剩餘參數允許我們將一個不定數量的參數表示為一個陣列。它使用三個點（...）作為語法，這些點稱為展開運算符。

## 定義

1. **允許接受零個或多個指定類型的參數**：剩餘參數可以接受任意數量的參數，即使是零個。
2. **一個函數（或方法）只能有一個剩餘參數**：在一個函數或方法中，只能定義一個剩餘參數。
3. **這個參數必須出現在參數列表的最後**：剩餘參數必須放在參數列表的最後，不能出現在中間或開頭。
4. **剩餘參數的類型是陣列型別**：在 TypeScript 中，剩餘參數的類型是一個陣列型別，可以是任何類型的陣列。

```typescript
function sum(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3)); // 輸出: 6
console.log(sum(1, 2, 3, 4, 5)); // 輸出: 15
```

在這個範例中，`sum` 函數使用剩餘參數來接受不定數量的 `number` 型別參數，並將它們累加。

# Arguments（參數）

在 JavaScript 和 TypeScript 中，每個函數都有一個名為 `arguments` 的內建物件。這個物件是一個類陣列物件，包含了函數被調用時所有傳入的參數。

```typescript
function logArguments() {
    for (let i = 0; i < arguments.length; i++) {
        console.log(arguments[i]);
    }
}

logArguments(1, 'a', true);
// 輸出:
// 1
// 'a'
// true
```

在這個範例中，我們使用 `arguments` 物件來訪問和列印所有傳入的參數。

# 差異比較

-   Rest Parameters：
    使用 `...` 語法，將不定數量的參數收集到一個陣列中，是正式的參數，可以有型別定義，並且是標準的陣列。

-   Arguments：
    是一個內建的類陣列物件，包含所有傳入的參數，但不是正式的陣列，並且不能有型別定義。

|          | Rest Parameters | Arguments                          |
| -------- | --------------- | ---------------------------------- |
| **語法** | 使用 `...` 語法 | 包含所有傳入的參數                 |
| **陣列** | 是標準的陣列    | 內建的類陣列物件，但不是正式的陣列 |
| **型別** | 可以有型別定義  | 不能有型別定義                     |


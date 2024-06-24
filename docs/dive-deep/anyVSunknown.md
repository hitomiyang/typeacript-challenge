# Any 任意型別

在 TypeScript 中，`any` 型別具有特殊的地位。當一個變數被定義為 `any` 型別時，TypeScript 會跳過該變數的型別檢查。因此，`any` 型別可以兼容所有的型別，無論是基礎型別（如數字、字串、布林值等），還是複雜型別（如陣列、物件、函式等）。此外，`any` 型別也可以賦值給任何其他型別。

## 範例

```typescript
let value: any;

// 可以接收任何型別賦值
value = true; // 布林型別，OK
value = 18; // 數字型別，OK
value = 'Hello World'; // 字串型別，OK
value = []; // 陣列型別，OK
value = {}; // 基礎物件型別，OK
value = null; // null型別，OK
value = undefined; // undefined型別，OK
value = new TypeError(); // Error物件，OK
value = Symbol('type'); // Symbol型別，OK
```

由於 TypeScript 對 `any` 型別不進行任何檢查，因此可以對 `any` 型別的變數進行任意操作。

## 範例

```typescript
let value: any;

value.length; // 求字串長度，OK
value.trim(); // 去除前後空格，OK
value(); // 執行函式，OK
new value(); // 創建物件，OK
value[0]; // 取陣列元素，OK
```

雖然 `any` 型別提供了極大的靈活性，但也容易導致編譯通過但實際運行時出錯的情況。因此，應盡量避免使用 `any` 型別。

# Unknown 未知型別

`unknown` 型別在 TypeScript 3.0 中引入，是 `any` 型別的更安全版本。

`unknown` 型別和 `any` 型別一樣，可以接受任何型別的賦值，但不能賦值給其他具體型別（除了 `any` 和 `unknown` 本身）。

## 範例

```typescript
let value: unknown;

value = true; // 布林型別，OK
value = 18; // 數字型別，OK
value = 'Hello World'; // 字串型別，OK
value = []; // 陣列型別，OK
value = {}; // 基礎物件型別，OK
value = null; // null型別，OK
value = undefined; // undefined型別，OK
value = new TypeError(); // Error物件，OK
value = Symbol('type'); // Symbol型別，OK
```

但若將 `unknown` 型別賦值給其他具體型別，則會產生錯誤。

## 範例

```typescript
let value: unknown;

let value1: unknown = value; // unknown型別，OK
let value2: any = value; // any型別，OK
let value3: boolean = value; // 布林型別，Error
let value4: number = value; // 數字型別，Error
let value5: string = value; // 字串型別，Error
let value6: object = value; // 物件型別，Error
let value7: any[] = value; // 空陣列，Error
let value8: void = value; // 空值，Error
```

當 `unknown` 型別的變數被操作屬性或方法時，也會產生錯誤。

```typescript
let value: unknown;

value.length; // Error
value.trim(); // Error
value(); // Error
new value(); // Error
value[0]; // Error
```

要操作 `unknown` 型別的屬性或方法，必須先進行型別檢測或型別斷言。

## 範例

```typescript
let isUnknown: unknown;

// 型別檢測
if (typeof isUnknown === 'number') {
    let value: number = isUnknown;
}

// 型別斷言
const value: unknown = 'Hello World';
const someString: string = value as string;
const otherString = someString.toUpperCase(); // "HELLO WORLD"
```


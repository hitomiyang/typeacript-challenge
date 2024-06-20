在 TypeScript 中，`extends` 是一個非常重要的關鍵字，有兩種主要的用途：一是用於約束泛型型別，二是用於條件型別中的型別檢查。這兩個用途幫助我們在開發中更靈活和精確地使用型別。

# 用於約束泛型型別：

`extends` 可以用於約束泛型型別，使得泛型型別必須符合某個特定的型別。這在定義函數、類或接口時非常有用，可以保證傳入的參數符合預期的型別。

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}
```

在這個例子中，`K extends keyof T` 表示 `K` 必須是 `T` 的鍵之一，這樣我們可以安全地訪問 `obj` 的屬性。

```typescript
type Person = {
    name: string;
    age: number;
};

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const person: Person = { name: 'Alice', age: 25 };

const name = getProperty(person, 'name'); // 正確
const age = getProperty(person, 'age'); // 正確
// const invalid = getProperty(person, "address"); // 錯誤，因為 "address" 不是 Person 的鍵
```

在這個例子中，`getProperty` 函數可以安全地訪問 `Person` 型別中的 `name` 和 `age` 屬性，而不能訪問不存在的 `address` 屬性。

---

# 用於條件型別：

`extends` 也可以在條件型別中用於檢查型別是否符合某個模式。條件型別使用類似於三元運算符的語法：`T extends U ? X : Y`。

```typescript
type IsString<T> = T extends string ? true : false;

type A = IsString<string>; // true
type B = IsString<number>; // false
```

在這個例子中，`IsString<T>` 用於檢查 `T` 是否是 `string` 型別。如果是，則返回 `true`，否則返回 `false`。

```typescript
type Flatten<T> = T extends any[] ? T[number] : T;

type A = Flatten<string[]>; // string
type B = Flatten<number>; // number
```

在這個例子中，`Flatten<T>` 用於將數組型別展平。如果 `T` 是數組，則返回數組元素的型別；否則，返回 `T` 本身的型別。

---

# 總結

`extends` 是 TypeScript 中的一個強大工具，既可以用於約束泛型型別，又可以在條件型別中進行型別檢查。這些功能使得我們可以更靈活和精確地定義和操作型別。


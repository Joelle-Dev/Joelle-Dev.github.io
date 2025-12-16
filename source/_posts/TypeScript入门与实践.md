---
title: TypeScript 入门与实践：从 JavaScript 到类型安全
date: 2025-12-15 21:30:00
tags:
  - TypeScript
  - 前端
  - 编程语言
---

## 前言

TypeScript 是 JavaScript 的超集，添加了静态类型系统。它不仅能帮助我们在开发阶段发现错误，还能提供更好的 IDE 支持和代码可维护性。本文将从基础到进阶，全面介绍 TypeScript 的使用。

## 一、TypeScript 简介

### 1.1 什么是 TypeScript

TypeScript 是由微软开发的开源编程语言，它是 JavaScript 的超集，意味着：

- ✅ 所有 JavaScript 代码都是有效的 TypeScript 代码
- ✅ 添加了静态类型系统
- ✅ 编译后生成纯 JavaScript 代码
- ✅ 支持最新的 ECMAScript 特性

### 1.2 为什么使用 TypeScript

**优势**：
- 🎯 **类型安全**：在编译时发现错误
- 💡 **更好的 IDE 支持**：自动补全、重构、导航
- 📚 **更好的文档**：类型即文档
- 🔧 **更容易重构**：类型系统帮助安全重构
- 👥 **团队协作**：类型定义让代码更易理解

**适用场景**：
- 大型项目
- 团队协作开发
- 需要长期维护的项目
- 复杂的业务逻辑

### 1.3 安装和配置

```bash
# 全局安装
npm install -g typescript

# 本地安装（推荐）
npm install --save-dev typescript

# 初始化 TypeScript 配置
npx tsc --init

# 编译 TypeScript 文件
tsc index.ts

# 监听模式
tsc --watch
```

## 二、基础类型

### 2.1 基本类型

```typescript
// 布尔值
let isDone: boolean = false;

// 数字
let count: number = 42;
let price: number = 99.99;

// 字符串
let name: string = "TypeScript";
let message: string = `Hello, ${name}!`;

// 数组
let list: number[] = [1, 2, 3];
let items: Array<number> = [1, 2, 3]; // 泛型语法

// 元组（Tuple）
let tuple: [string, number] = ["hello", 10];

// 枚举（Enum）
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;

// 字符串枚举
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}

// Any（任意类型）
let notSure: any = 4;
notSure = "maybe a string";
notSure = false;

// Void（无返回值）
function warnUser(): void {
  console.log("This is a warning");
}

// Null 和 Undefined
let u: undefined = undefined;
let n: null = null;

// Never（永不存在的值）
function error(message: string): never {
  throw new Error(message);
}

// Object（非原始类型）
let obj: object = { x: 0 };
```

### 2.2 类型推断

```typescript
// TypeScript 可以自动推断类型
let x = 3; // x 的类型是 number
let y = "hello"; // y 的类型是 string

// 函数返回类型推断
function add(a: number, b: number) {
  return a + b; // 返回类型推断为 number
}
```

### 2.3 类型断言

```typescript
// 方式一：尖括号语法
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

// 方式二：as 语法（推荐，在 JSX 中必须使用）
let strLength2: number = (someValue as string).length;

// 非空断言
let element = document.getElementById("myDiv")!;
element.innerHTML = "Hello";
```

## 三、接口（Interface）

### 3.1 基础接口

```typescript
// 定义接口
interface User {
  name: string;
  age: number;
  email?: string; // 可选属性
  readonly id: number; // 只读属性
}

// 使用接口
function createUser(user: User): User {
  return {
    name: user.name,
    age: user.age,
    email: user.email,
    id: Date.now(),
  };
}

const user: User = {
  name: "John",
  age: 30,
  id: 1,
};
```

### 3.2 接口扩展

```typescript
// 接口继承
interface Animal {
  name: string;
}

interface Dog extends Animal {
  breed: string;
}

const myDog: Dog = {
  name: "Buddy",
  breed: "Golden Retriever",
};

// 多接口继承
interface Shape {
  color: string;
}

interface PenStroke {
  penWidth: number;
}

interface Square extends Shape, PenStroke {
  sideLength: number;
}
```

### 3.3 函数类型接口

```typescript
// 定义函数接口
interface SearchFunc {
  (source: string, subString: string): boolean;
}

// 使用函数接口
let mySearch: SearchFunc;
mySearch = function (src: string, sub: string): boolean {
  return src.search(sub) > -1;
};
```

### 3.4 索引签名

```typescript
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

interface NumberDictionary {
  [index: string]: number;
  length: number; // 可以，length 是 number 类型
  name: string; // 错误，name 的类型与索引类型返回值的类型不匹配
}
```

## 四、类（Class）

### 4.1 基础类

```typescript
class Greeter {
  greeting: string;

  constructor(message: string) {
    this.greeting = message;
  }

  greet() {
    return "Hello, " + this.greeting;
  }
}

let greeter = new Greeter("world");
```

### 4.2 访问修饰符

```typescript
class Animal {
  public name: string; // 公共（默认）
  private age: number; // 私有
  protected species: string; // 受保护
  readonly id: number; // 只读

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
    this.species = "unknown";
    this.id = Date.now();
  }
}

class Dog extends Animal {
  constructor(name: string, age: number) {
    super(name, age);
    this.species = "Canine"; // 可以访问 protected
    // this.age = 5; // 错误，不能访问 private
  }
}
```

### 4.3 抽象类

```typescript
abstract class Animal {
  abstract makeSound(): void; // 抽象方法

  move(): void {
    console.log("roaming the earth...");
  }
}

class Dog extends Animal {
  makeSound(): void {
    console.log("Woof! Woof!");
  }
}
```

### 4.4 静态属性和方法

```typescript
class MathHelper {
  static PI: number = 3.14159;

  static calculateArea(radius: number): number {
    return this.PI * radius * radius;
  }
}

console.log(MathHelper.PI);
console.log(MathHelper.calculateArea(5));
```

### 4.5 Getter 和 Setter

```typescript
class Employee {
  private _fullName: string = "";

  get fullName(): string {
    return this._fullName;
  }

  set fullName(newName: string) {
    if (newName && newName.length > 0) {
      this._fullName = newName;
    } else {
      throw new Error("Invalid name");
    }
  }
}

let employee = new Employee();
employee.fullName = "John Doe";
console.log(employee.fullName);
```

## 五、函数

### 5.1 函数类型

```typescript
// 函数声明
function add(x: number, y: number): number {
  return x + y;
}

// 函数表达式
let myAdd: (x: number, y: number) => number = function (
  x: number,
  y: number
): number {
  return x + y;
};

// 箭头函数
let multiply = (x: number, y: number): number => x * y;
```

### 5.2 可选参数和默认参数

```typescript
function buildName(firstName: string, lastName?: string): string {
  if (lastName) {
    return firstName + " " + lastName;
  }
  return firstName;
}

function buildName2(firstName: string, lastName: string = "Smith"): string {
  return firstName + " " + lastName;
}
```

### 5.3 剩余参数

```typescript
function buildName3(firstName: string, ...restOfName: string[]): string {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName3("Joseph", "Samuel", "Lucas", "MacKinzie");
```

### 5.4 函数重载

```typescript
function pickCard(x: { suit: string; card: number }[]): number;
function pickCard(x: number): { suit: string; card: number };
function pickCard(x: any): any {
  if (typeof x == "object") {
    return Math.floor(Math.random() * x.length);
  } else if (typeof x == "number") {
    return { suit: "spades", card: x % 13 };
  }
}
```

## 六、泛型（Generics）

### 6.1 基础泛型

```typescript
// 泛型函数
function identity<T>(arg: T): T {
  return arg;
}

let output = identity<string>("myString");
let output2 = identity("myString"); // 类型推断

// 泛型接口
interface GenericIdentityFn<T> {
  (arg: T): T;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

### 6.2 泛型类

```typescript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```

### 6.3 泛型约束

```typescript
interface Lengthwise {
  length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}

loggingIdentity("hello"); // OK
loggingIdentity([1, 2, 3]); // OK
// loggingIdentity(3); // Error
```

### 6.4 在泛型约束中使用类型参数

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };
getProperty(x, "a"); // OK
// getProperty(x, "m"); // Error
```

## 七、高级类型

### 7.1 联合类型（Union Types）

```typescript
let value: string | number;
value = "hello";
value = 42;
// value = true; // Error

// 类型守卫
function padLeft(value: string, padding: string | number) {
  if (typeof padding === "number") {
    return Array(padding + 1).join(" ") + value;
  }
  if (typeof padding === "string") {
    return padding + value;
  }
  throw new Error(`Expected string or number, got '${padding}'.`);
}
```

### 7.2 交叉类型（Intersection Types）

```typescript
interface Colorful {
  color: string;
}

interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;

let cc: ColorfulCircle = {
  color: "red",
  radius: 10,
};
```

### 7.3 类型别名（Type Aliases）

```typescript
type StringOrNumber = string | number;
type Text = string | { text: string };
type NameLookup = Dictionary<string, Person>;
type Callback<T> = (data: T) => void;
type Pair<T> = [T, T];
type Coordinates = Pair<number>;
type Tree<T> = T | { left: Tree<T>; right: Tree<T> };
```

### 7.4 字面量类型

```typescript
type Easing = "ease-in" | "ease-out" | "ease-in-out";

function animate(dx: number, dy: number, easing: Easing) {
  // ...
}

animate(0, 0, "ease-in");
// animate(0, 0, "uneasy"); // Error
```

### 7.5 keyof 和 typeof

```typescript
// keyof 操作符
interface Person {
  name: string;
  age: number;
}

type PersonKeys = keyof Person; // "name" | "age"

// typeof 操作符
let s = "hello";
let n: typeof s; // string

// 结合使用
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

### 7.6 映射类型（Mapped Types）

```typescript
// 将所有属性变为可选
type Partial<T> = {
  [P in keyof T]?: T[P];
};

// 将所有属性变为只读
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

// 使用示例
interface Todo {
  title: string;
  description: string;
}

type PartialTodo = Partial<Todo>;
type ReadonlyTodo = Readonly<Todo>;
```

### 7.7 条件类型（Conditional Types）

```typescript
type NonNullable<T> = T extends null | undefined ? never : T;

type TypeName<T> = T extends string
  ? "string"
  : T extends number
  ? "number"
  : T extends boolean
  ? "boolean"
  : T extends undefined
  ? "undefined"
  : T extends Function
  ? "function"
  : "object";
```

## 八、模块和命名空间

### 8.1 模块

```typescript
// math.ts
export function add(x: number, y: number): number {
  return x + y;
}

export function subtract(x: number, y: number): number {
  return x - y;
}

// 默认导出
export default class Calculator {
  // ...
}

// app.ts
import Calculator, { add, subtract } from "./math";
import * as math from "./math";
```

### 8.2 命名空间

```typescript
namespace Validation {
  export interface StringValidator {
    isAcceptable(s: string): boolean;
  }

  const lettersRegexp = /^[A-Za-z]+$/;
  const numberRegexp = /^[0-9]+$/;

  export class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string) {
      return lettersRegexp.test(s);
    }
  }

  export class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
      return s.length === 5 && numberRegexp.test(s);
    }
  }
}
```

## 九、装饰器（Decorators）

### 9.1 类装饰器

```typescript
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@sealed
class BugReport {
  type = "report";
  title: string;

  constructor(t: string) {
    this.title = t;
  }
}
```

### 9.2 方法装饰器

```typescript
function enumerable(value: boolean) {
  return function (
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    descriptor.enumerable = value;
  };
}

class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }

  @enumerable(false)
  greet() {
    return "Hello, " + this.greeting;
  }
}
```

## 十、实用工具类型

### 10.1 内置工具类型

```typescript
// Partial<T> - 所有属性变为可选
interface Todo {
  title: string;
  description: string;
}
type PartialTodo = Partial<Todo>;

// Required<T> - 所有属性变为必需
type RequiredTodo = Required<PartialTodo>;

// Readonly<T> - 所有属性变为只读
type ReadonlyTodo = Readonly<Todo>;

// Pick<T, K> - 选择部分属性
type TodoPreview = Pick<Todo, "title" | "description">;

// Omit<T, K> - 排除部分属性
type TodoInfo = Omit<Todo, "title">;

// Record<K, T> - 创建对象类型
type PageInfo = Record<"home" | "about" | "contact", { title: string }>;

// Exclude<T, U> - 从 T 中排除 U
type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"

// Extract<T, U> - 从 T 中提取 U
type T1 = Extract<"a" | "b" | "c", "a" | "f">; // "a"

// NonNullable<T> - 排除 null 和 undefined
type T2 = NonNullable<string | number | undefined>; // string | number

// ReturnType<T> - 获取函数返回类型
type T3 = ReturnType<() => string>; // string

// Parameters<T> - 获取函数参数类型
type T4 = Parameters<(s: string) => void>; // [string]
```

## 十一、TypeScript 配置

### 11.1 tsconfig.json 常用配置

```json
{
  "compilerOptions": {
    // 目标版本
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],

    // 模块解析
    "moduleResolution": "node",
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },

    // 类型检查
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true,

    // 额外检查
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,

    // 输出
    "outDir": "./dist",
    "rootDir": "./src",
    "removeComments": true,
    "sourceMap": true,

    // 实验性功能
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,

    // 其他
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

## 十二、React + TypeScript

### 12.1 组件类型

```typescript
import React from "react";

// 函数组件
interface Props {
  name: string;
  age?: number;
}

const Greeting: React.FC<Props> = ({ name, age }) => {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      {age && <p>You are {age} years old.</p>}
    </div>
  );
};

// 或者不使用 React.FC
const Greeting2 = ({ name, age }: Props) => {
  return <div>Hello, {name}!</div>;
};
```

### 12.2 Hooks 类型

```typescript
import { useState, useEffect, useRef } from "react";

function Counter() {
  const [count, setCount] = useState<number>(0);
  const [name, setName] = useState<string>("");
  const inputRef = useRef<HTMLInputElement>(null);

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <div>
      <input ref={inputRef} value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
    </div>
  );
}
```

### 12.3 事件类型

```typescript
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  console.log(e.target.value);
};

const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
  e.preventDefault();
  console.log("clicked");
};

const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  // ...
};
```

## 十三、最佳实践

### 13.1 类型定义建议

```typescript
// ✅ 好的做法：使用接口定义对象
interface User {
  id: number;
  name: string;
}

// ✅ 好的做法：使用类型别名定义联合类型
type Status = "pending" | "approved" | "rejected";

// ✅ 好的做法：使用 const 断言
const colors = ["red", "green", "blue"] as const;
type Color = typeof colors[number]; // "red" | "green" | "blue"

// ❌ 避免：过度使用 any
function processData(data: any) {
  // ...
}

// ✅ 好的做法：使用 unknown 或具体类型
function processData(data: unknown) {
  if (typeof data === "string") {
    // ...
  }
}
```

### 13.2 项目结构

```
src/
├── types/          # 类型定义
│   ├── index.ts
│   └── user.ts
├── utils/          # 工具函数
├── components/     # 组件
├── hooks/          # 自定义 Hooks
└── api/            # API 调用
```

## 十四、常见问题

### 14.1 类型错误处理

```typescript
// 类型断言
const element = document.getElementById("myDiv") as HTMLDivElement;

// 类型守卫
function isString(value: unknown): value is string {
  return typeof value === "string";
}

if (isString(value)) {
  // value 在这里是 string 类型
}
```

### 14.2 第三方库类型

```typescript
// 安装类型定义
npm install --save-dev @types/lodash

// 如果没有类型定义，可以声明模块
declare module "some-library" {
  export function doSomething(): void;
}
```

## 总结

TypeScript 为 JavaScript 添加了强大的类型系统，能够：

1. **提高代码质量**：在编译时发现错误
2. **改善开发体验**：更好的 IDE 支持
3. **增强可维护性**：类型即文档
4. **便于重构**：类型系统帮助安全重构

学习 TypeScript 的关键是：
- 从基础类型开始
- 逐步学习高级特性
- 在实际项目中应用
- 充分利用类型系统

记住：**TypeScript 是工具，不是负担**。合理使用类型系统，让代码更安全、更易维护。

## 参考资源

- [TypeScript 官方文档](https://www.typescriptlang.org/docs/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)


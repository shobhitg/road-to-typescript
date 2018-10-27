# $TupleMap&lt;T, F&gt;

Flow's [`$TupleMap<T, F>`](https://flow.org/en/docs/types/utilities/#toc-tuplemap) takes a a tuple (or array) type `T` and a function type `F` and returns the type obtained by applying `F` to each element of `T`.

Like the [`$ObjMap<T, F>`](toc-objmap.md) type, this cannot be directly implemented in TypeScript. There is an open issue ([#1213](https://github.com/Microsoft/TypeScript/issues/1213)) in the TypeScript repo which, once resolved, will make it possible to directly implement this type.

The desired types can still be achieved through writing types similar to the types in `$ObjMap<T, F>`.

Flow example ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQiSgGIFcB2BjALgJaxah4CeADgKakAWAhnqfQNZUDOo9oABgBQBKUAF4AfKABqPLlgAmoAE5U8GBVk7ceU0HwJ5OSlWtKUqAxOWqgAogA88C+vgBKy1VgAqpkaAA8E0T5BEXEJITFJZEhsfCISBWxfAEEAGlAASQAuUCSFRzJfYIik0UC9KgVs9IFsgBIPDApoKgBZegpfdLS7Byc8VyNPU3EAb0RQRTdjcoUAOgBbdr5IEgiVwQEAbkQAX2QcYnYmejyfAG0i8QBySFhYK7TL0CuAIxOrgF1tvgSsPhOFAIzgAGD7ZI4KAhYADmW1AqAA8gBpRA-bD-PJAgCMYNAEKhsM28LAyNRvwxgLOOOyLzuzXoWDhqGseVgCiAA))

```javascript
// @flow

// Function type that takes a `() => V` and returns a `V` (its return type)
type ExtractReturnType = <V>(() => V) => V

function run<A, I: Array<() => A>>(iter: I): $TupleMap<I, ExtractReturnType> {
  return iter.map(fn => fn());
}

const arr = [() => 'foo', () => 'bar'];
(run(arr)[0]: string); // OK
(run(arr)[1]: string); // OK
(run(arr)[1]: boolean); // Error
```

TypeScript example ([playground][1])

```typescript
// Avoid immediate errors where T might not be a function
type LaxReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

function run<I extends Array<() => any>>(iter: I): { [K in keyof I]: LaxReturnType<I[K]> } {
  return iter.map(fn => fn()) as any;
}

const arr: [() => string, () => number] = [() => 'foo', () => 1];
const first: string = run(arr)[0];
const second: number = run(arr)[1];
const fail: boolean = run(arr)[1];
```

<!-- Note: This uses the typescriptlang playground because it requires a more recent TS version than is available on AgentCooper's -->
[1]: https://www.typescriptlang.org/play/index.html#src=%2F%2F%20Avoid%20immediate%20errors%20where%20T%20might%20not%20be%20a%20function%0D%0Atype%20LaxReturnType%3CT%3E%20%3D%20T%20extends%20(...args%3A%20any%5B%5D)%20%3D%3E%20infer%20R%20%3F%20R%20%3A%20never%3B%0D%0A%0D%0Afunction%20run%3CI%20extends%20Array%3C()%20%3D%3E%20any%3E%3E(iter%3A%20I)%3A%20%7B%20%5BK%20in%20keyof%20I%5D%3A%20LaxReturnType%3CI%5BK%5D%3E%20%7D%20%7B%0D%0A%20%20return%20iter.map(fn%20%3D%3E%20fn())%20as%20any%3B%0D%0A%7D%0D%0A%0D%0Aconst%20arr%3A%20%5B()%20%3D%3E%20string%2C%20()%20%3D%3E%20number%5D%20%3D%20%5B()%20%3D%3E%20'foo'%2C%20()%20%3D%3E%201%5D%3B%0D%0Aconst%20first%3A%20string%20%3D%20run(arr)%5B0%5D%3B%0D%0Aconst%20second%3A%20number%20%3D%20run(arr)%5B1%5D%3B%0D%0Aconst%20fail%3A%20boolean%20%3D%20run(arr)%5B1%5D%3B

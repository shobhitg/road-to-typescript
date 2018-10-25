# $ElementType&lt;T, K&gt;

In Flow, [`$ElementType<T, K>`](https://flow.org/en/docs/types/utilities/#toc-elementtype) is the union of all types inside an array, tuple, or object type `T` that match the given key type `K`.

Unlike Flow, TypeScript generally does not require separate types for `$PropertyType<T, k>` and `$ElementType<T, K>`, the desired type can be achieved with `T[K]`. However, some special treatment is required for Tuples.

Flow Example ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQiSgKoGcCWA7A5qLAEYBWApgMYAuGAXIlQJ4AOZoA8qaALygDeiUKBwBDALZlaoDFQBOuPABpBoEXknCArmKJlZygL6IAFAHIAUrBympAEgCi0MhJxUAKizIAeTiUWhTUQlTAD4ASgBuEwAWACY7R2cyVw9WH1J-UzUyUMiTOU0NBycXd090vwCgnPCI0FQAOVhWfwADatbQLAxhWCpVUCJYWCcRHHzZQoSS5LK030y+gAs9XLrG5rJ-ZllN2SZQVuW9Tu7e-twOUmRUTAVQKk1mJzoGT1A3J6ceUABtIZGZDG-hk8nwAF0osYCkVEqVUt5Ps8tqAAAy1EymSDDGygYpJFLlJFOfwARgxZiIIlkuPx8KJXxRsVq9TATRaoAoY1M-REFAoZAwPWYsGwVCwVlAsSAA))

```javascript
// @flow

// Using objects:
type Obj = {
  name: string,
  age: number,
}
('Jon': $ElementType<Obj, 'name'>);
(42: $ElementType<Obj, 'age'>);
(true: $ElementType<Obj, 'name'>); // Nope, `name` is not a boolean
(true: $ElementType<Obj, 'other'>); // Nope, property `other` is not in Obj

// Using tuples:
type Tuple = [boolean, string];
(true: $ElementType<Tuple, 0>);
('foo': $ElementType<Tuple, 1>);
('bar': $ElementType<Tuple, 2>); // Nope, can't access position 2
```

TypeScript Example ([playground](https://agentcooper.github.io/typescript-play/#code/PTAEFUGcEsDsHNQHsBGArApgYwC6QFwBQOAngA4agDy6oAvKAN6GiiwCGAthvqJDgCc48ADQtQ7eDzYBXTigwCxAX0LFylACoyyAGwwBRfd1g5NGgDybQGAB44MsACaQJsEgG0AuiNABpAD56cVYAWgBGG3tHF39QAH5QTQ9YOQUBL1BeEFAACXZnfSSdfSMMEzNLTV9U+UUAkKSPAHJ9BBwAC2bMuwdnVwAGBLYMADdFLMa-KL7YocTkgcyiVlZk1sd4Tu6ZmNdIxNgxiZXV6d690AOm8OXG9batrp7o-tAAJmGj8YFJ1biLm9PgsPO87v8Hptti9Zq4AMxfY6-U6sc6vWIIkFw5agHIAOgJUTIBScElcAgwAEcZNAKU5Gt9FGpSBRQGUKuYKFZfIFghDdm8Cp5MgsSoZjI5Klzqv4giiAejXABrDAkJAAMySw2SfnBq0ZAjUWCQsH4oDQJt47MlnIwFhoaF8zQ43GaQQYzQAUibmgBuQjG004CRSK0S0y2+3oJ2SDBu+igAAs739gbN6vY0F0YfKNssDqdLrj7tAghkGF9uLAADkkBRfAADIsN0DQVywJDB9igFBIJD6AoBk3pzO6d45jn56OgZqdjqKeMMMsVqugWv10BkAR1xSkUANueKFtttid1uwajoNQ5KDCUtigjqVnaPSUBgeXv9jAFXz8IQILxU2HYN1VpfgJzzaUxV8AYS2XICgz4bATScCCIyqaCrhLZp1T7P0h0QjMszhNCpTtF99F8d5sJQdgBD9Vd1wwXwsAKZouywLAMEgVwyCQGAcGgE0PkIIA))

```typescript
// Using objects:
type Obj = {
  name: string,
  age: number,
}

type TupleElementType<T extends any[], K> =
    -1 extends K ? T[number] : // Handle TupleElementType<T, number>
    T['length'] extends 0 ? never :
    K extends 0 ? T[0] :
    T['length'] extends 1 ? never :
    K extends 1 ? T[1] :
    T['length'] extends 2 ? never :
    K extends 2 ? T[2] :
    T['length'] extends 3 ? never :
    K extends 3 ? T[3] : // ... expand as required
    never

type ElementType<T, K> =
    T extends any[] ? TupleElementType<T, K> :
    K extends keyof T ? T[K] :
    never

const jon: ElementType<Obj, 'name'> = 'Jon';
const age: ElementType<Obj, 'age'> = 42;
const fail: ElementType<Obj, 'name'> = true; // Nope, `name` is not a boolean
const fail2: ElementType<Obj, 'other'> = true; // Nope, property `other` is not in Obj

// Using tuples:
type Tuple = [boolean, string];
const first: ElementType<Tuple, 0> = true;
const second: ElementType<Tuple, 1> = 'foo';
const fail3: ElementType<Tuple, 2> = 'bar'; // Nope, can't access position 2
```

# $Call&lt;F&gt;

Flow's [`$Call<F>`](https://flow.org/en/docs/types/utilities/#toc-call) takes a function type and calls it, returning the resulting type.

This functionality does not have a one-to-one mapping in TypeScript. However, TypeScript's [conditional types](../advanced-usecases/conditional-types.md) provide an alternative which matches Flow's `$Call<F>` in flexibility in most cases.

The one case in which Flow's `$Call<F>` is definitively more useful than TypeScript's conditional types is when dealing with generic functions. Example 4 shows a case where Flow does not require writing a separate type, but TypeScript cannot produce the desired type from the function type.

Flow example ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQiSgCoEMDWBTAzqBgHaiwBGAVjgMYAuotAngA44A0oATjrQK6dECtABY4GLMbEigAlrQIADZp1jMFoXI0RNWoAKIAPWpwx0ACiuZoJoALygAPGgB8ACgDey1QC50AXwBKO2d0AG5tGwB5SjtQT0tfIl4AWzIcTj9wnTELVWtdewASAGEMaGgHQ2NTWlyrCQ5oimdQ0FBUUvLQBSqTc0t8nHV4OWFupvUMAmJCTgBzFJwiWgjdADlVMSLOit6ausGOdyJNxJS0jJa21D1OFU5fDHnF5dAAE1h8IgByemSMWjUMYKCYAOmQrgArL4DhIAq1UJEANKIVzGXg4GEDOGtdpgW73LF5GwyaagJKpdKo6GgDaseHXfF3WCcIA))

```javascript
// @flow

// Takes an object type, returns the type of its `prop` key
type ExtractPropType = <T>({prop: T}) => T;
type Obj = {prop: number};
type PropType = $Call<ExtractPropType, Obj>;  // Call `ExtractPropType` with `Obj` as an argument
type Nope = $Call<ExtractPropType, {nope: number}>;  // Error: argument doesn't match `Obj`.

(5: PropType); // OK
(true: PropType);  // Error: PropType is a number
(5: Nope);  // Error
```

TypeScript example ([playground](https://agentcooper.github.io/typescript-play/#code/PTAEBUEMGsFMGdSQHagPYCMBWsDGAXUfATwAdYAaUAJ1nwFdrlF8ALWIsjtAM1AEt8iAAalqaUsNBxiAKBLlQAUQAe+apAIAFcaXBcAPOFCw1sZABNEAb1BiJALiTJioAL4A+UAF4IJs5Y2drpO-Mg8sNSgAKruoAD8MaBOyLAAbpEA3LLyXKAA8tg+oNb2pCn0ALYYkW7ZChw6EvqKvqrqmvhNeoaFWB71eQByEhxtahraui2wBtbIoxXVtQOgoCDK1OLUObhozISQTt0zxQCsmetg+dCyewegPJD8ADbH03m+6vSwlxtKWzQ1HezTy-EQkFAyCqNR293ghCerwATE4Rq1QGcrpttmjRgJEKkMtQgA))

```typescript
// Takes an object type, returns the type of its `prop` key
type ExtractPropType<T extends { prop: any }> = T extends { prop: infer U } ? U : never;

type Obj = {prop: number};
type PropType = ExtractPropType<Obj>;
type Nope = ExtractPropType<{nope: number}>; // Error

const a: PropType = 5; // Ok
const fail: PropType = true; // Error: PropType is a number
const fail2: Nope = 5 // Error: Nope is never
```

Flow example 2 ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQiSgCoEMDWBTAzqBqJAK4B2AxgC4CWsZoVAngA44A0hZAJqAE44qJPmQI0qBAUJGNWOFGDQALGmIIk8OUtFA1IoJrBKh4GMlUaxQAc0GMlOfoOENmbULH15YAW0elKWnoTcSUjCwxqEgxoaCZQChjoGjJrXQiLPnJaPwA6RDdHAFEADyo+SKoAJWcRNDlQAF5QAB4qgD4ACk6ASib20Cq+xoGqgG4ChoAxBmbe-tAyEh8AIxw+CcLB2rJ692aAEgBhJJbS8sqa6V25ThmB1BPY0AADc4rqK5c9nBeQqiUrxmfwwBDMhD41mWOHMyE6AFYAFzba4-HpjUCgVAAeQA0ohOuUSDhkV86nJ0ZjUEU+HxYHxSTsfrowYtlms+EA))

```javascript
// @flow

// Takes a function type, and returns its return type
// This is useful if you want to get the return type of some function without actually calling it at runtime.
type ExtractReturnType = <R>(() => R) => R;
type Fn = () => number;
type ReturnType = $Call<ExtractReturnType, Fn> // Call `ExtractReturnType` with `Fn` as an argument

(5: ReturnType);  // OK
(true: ReturnType);  // Error: ReturnType is a number
```

TypeScript example 2 ([playground](https://agentcooper.github.io/typescript-play/#code/PTAEBUAsEsGdTqARgV2gGwC4IHa4gJ4AOApgMoDGATtEdgIbwAGASiZilTuMSU6PRwATUJEYDQsdNADmkTOgKgh0AGaqSVEjmyxZOehy0A6AFCZeoAKIAPTFXoVMbI914AeAGKgSd7UPgACmMQ+ioZWAAuARwCAG0AXQBKUABeAD4YgkzU0G9fTH8gkOMwiOjBeOS0zOgcDSpQFlAAfibQaJwSADdNc0tPPFzAlIzQHBQAWyRNAG5+0lAAWQIXTjdF3Nt7R2d2dZ5SLxx001MKAHscWAZolbWuQ5I00ABWWdAQUAB5AGtzq43UCqegYO6rfaPSy5ewoEgfL5WKhUC5UaIPDbPRD0cZTGZUIA))

```typescript
// This is built in in TypeScript as `ReturnType` and has a slightly different signature.
type ExtractReturnType<F extends (...args: any[]) => any> = F extends (...args: any[]) => infer R ? R : never
type Fn = () => number;
type MyReturnType = ExtractReturnType<Fn>

const a: MyReturnType = 5; // Ok
const fail: MyReturnType = true; // Error: ReturnType is a number
```

Flow example 3 ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQiSgKIA8AuAnAhgMZYCWAdgOagAmApjQA7QCeopNAzljVaFk-RwBciPgNAA5DlyoB5AEYArUAF5QAbwA+iUKADUnPFgCu7QaAD8pIwFs5NHABpteqobxnzAEgBKNPLNJmAEEcfCYAHk1nHV1IWFgPKJ1kmLk8HDMrW3snFI0AX1zQAoA+JwKAbmRUAElIUCZYI1B4PFJpXlhQGmx8Il4ACxpefmG4nFAAAzScSYcGptACJugeE2HPAGE8aGhhUWGAIXSAFVGVUC2d6HDncJOSgAo1aJc3Dx8-AODQvAiXlIxOIJCwAwGpdIeE6vHSFV75MrOfIAShUJVAJyKkk43HkCicJSqiEeAFYzMccGcBMiqo9cEYaOTTqMaTpUGhQrAMlMZpNQMR2KxYFhQHhQHJ4tA-KQgA))

```javascript
// @flow

// Extracting deeply nested types:
type NestedObj = {|
  +status: ?number,
  +data: ?$ReadOnlyArray<{|
    +foo: ?{|
       +bar: number,
    |},
  |}>,
|};

// If you wanted to extract the type for `bar`, you could use $Call:
type BarType = $Call<
  <T>({
    +data: ?$ReadOnlyArray<{
      +foo: ?{
        +bar: ?T
      },
    }>,
  }) => T,
  NestedObj,
>;

(5: BarType);
(true: BarType);  // Error: `bar` is not a boolean
```

TypeScript example 3 ([playground](https://agentcooper.github.io/typescript-play/#code/PTAEFEA8BcCcEMDG0CWA7A5qAJgU1wA4A2AnqGrgM7S7ajQkFUBcAUA06AHJU3YDyAIwBWoALygA3q1Chq8aAFdKAfmblFAW0G5YAGhk4F8NaABKueNgD2aUgEFYCEgB5ps2QDNr10+48egvCw6mhaOrCGsgC+htEAfKzRANys7Iy4EDAIyABCwQAqGS4F8eKgBaC4MLho2JRSRtAm6hZWtg5O8K6SoN6+6r1BIaDonrqgAKqg0QkzoCpToKG4AG666Zz5sEWcElBwSNDbu7guPNS0QsKJrIi21KDw6icZ5QCsqfdoj57wKEQXoU3hI4IpcMlQCAIE5rCMAAbDeGjBpoazQJ6gQQ+IiWNBAA))

```typescript
// Extracting deeply nested types:
type NestedObj = {
  status?: number,
  data?: ReadonlyArray<{
    foo?: {
       bar: number
    }
  }>
};

type ExtractBarType<T> = T extends { data?: ReadonlyArray<{ foo?: { bar: infer U }}> } ? U : never
type BarType = ExtractBarType<NestedObj>

const a: BarType = 5;
const fail: BarType = true; // Error: `bar` is not a boolean
```

Flow example 4 ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQiSgOIFMAuWCWA7Ac1ACdsBXE-ULATwAcMBnALkUnPwGM9ZrDsAMVwkmWAGoBDaOQwAecQD4AFAFtJ9FqACyGuWJIFCAGlBKAlFoD840AG9EoUJFglQyrnzGgA2gGsMWlMAN2lZAF1QWEhQdXoAOgx8LENmZXNze0cnUgoqUFCZDABubIBfbLIsSmp8cmhoUoqUMABVJiNQABIAYWloU3gMUC5JfmwaAAthyR5yaVzq-LpGKJisaedOHlw+UEkAI1hgjEHcDdhyLBH+zvP965JOPFUMNhXhqSLQAF5uvoacg+0VAAiwwlEEjCpx0egMRlMdVUBwwJEUilKiGUAFYtF9ZOZSsoUrI8dDCU5UABREgkVxk764Jj7UAAAyRKJIrOQLVAAHVhp56gATUFJVHSXAAL2G9w4JA2qPeDGGmCwuno+Pk2kUv2yvX6QJVILBELEWtMOsxOK0ao1WrkGv0KQRoA5qPRhKxJLe6Gw9uhjrhLqIpiOsGgGDGnqJPtt-o0Dqd8NDbvIyI9igpoGptPpBWhoCZLPZ6c5rKAA))

```javascript
// @flow

// Getting return types:
function getFirstValue<V>(map: Map<string, V>): ?V {
  for (const [key, value] of map.entries()) {
    return value;
  }
  return null;
}

// Using $Call, we can get the actual return type of the function above, without calling it at runtime:
type Value = $Call<typeof getFirstValue, Map<string, number>>;

(5: Value);
(true: Value);  // Error: Value is a `number`


// We could generalize it further:
type GetMapValue<M> =
  $Call<typeof getFirstValue, M>;

(5: GetMapValue<Map<string, number>>);
(true: GetMapValue<Map<string, boolean>>);
(true: GetMapValue<Map<string, number>>);  // Error: value is a `number`
```

TypeScript example 4 ([playground](https://agentcooper.github.io/typescript-play/#code/PTAEFUDsDMHsCcAuBXSBDRBTANgTwDSgAGiuADprNKAOaaIBiAlvAM6IBqa2ymoAvKAA8AFQB8ACgC2aMgC5QAWVlD28JpBqEOYgJQKOoAD6hIybNiIAoEKBEALZK1AB3PgGM0kAOSJQG6Ex4UER7PlIKVyZQ2GQ-eEwXdUQNGhCw0FYmGnQUBIA6QtAAIzj-RG9nTDQsoJDYWga1NGz7RBswRAbQvjpIIKZ3UAA3IKzYSHyrCL4AcXplMi4eTCFFUEwADyxIABNnRaEvAlBjsTEBJQ3tzD2DlWPCALqdUAB+UEMFftH4K2nyHxlrxLvNEItgatDmpUoQzFJikFzgBuf7uCbsU4GbggwQAVlR6MgmOgLWw2JWl0Q8F4yNAtgAovB4AgKSCmM54Yi-kSSWSAEwKMEQnFQlRcoKEADeWMy1NSAF9zpcZWgFABGUAK+lgJks+AKND+ZwwzRWIA))

```typescript
// Unfortunately, `typeof getFirstValue = <T>(map: Map<string, V>): V | null`
// Thus we can't infer the type without rewriting the signature... but it's easier to go straight
// to the generic version.
type GetMapValue<M extends Map<any, any>> = M extends Map<any, infer V> ? V : never

type Value = GetMapValue<Map<string, number>>;

const a: Value = 5;
const fail: Value = true; // Error: Value is number
const fail2: GetMapValue<Map<number, { a: string}>> = { a: 1 } // Error: a is string
```

# $NonMaybeType&lt;T&gt;

The `$NonMaybeType<T>` helper removes `null` and `undefined` from a type.

TypeScript can achieve this by using the built in `Exclude<T, U>` type that removes types from `T` that are assignable to `U`.

Flow example ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQAXAngBwKagLIENUAjTAOTwFtsBeUAfgGdkAnASwDsBzAbhQ2zMqgaAEhKw2+IpgAqfADyTiAzAD4eiABQByAOJ5CrTNC0AuXASUVMASi6gQoAPIBrTWwCu0aGcWkrt+zAXTV19Q2MzZQCHYI0PL0j-OwcAUSYmWCYAQlB46FAAYzw2LWRQYlBitlhkPGRMABNK+lBlcswi93p+K1AWFuqyvFByC2w0LCA))

```javascript
// @flow
type MaybeName = ?string;
type Name = $NonMaybeType<MaybeName>;

('Gabriel': MaybeName); // Ok
(null: MaybeName); // Ok
('Gabriel': Name); // Ok
(null: Name); // Error! null can't be annotated as Name because Name is not a maybe type
```

TypeScript example ([playground](https://agentcooper.github.io/typescript-play/#code/C4TwDgpgBAcg9gOwLIEMQCMIBVwQDxYB8UAvFAKIAeAxgDYCuAJvlgDRQL221QA+U9BMwBmASwQRGhAFDTQkKKgwQYKALbQyAZ2AAncQHM+AoRDETGxztzm5Y6zbERLMOSHhcqHM6bQjAoFAAuRTRMVQ1SKAByAHEUdH0IWmioAHo0qAB5AGtffyh0EM8Ix2seDOy8vwDqENKouISklPTM3PyAxnqHKPK2il1dOF1pIA))

```typescript
type NonMaybeType<T> = Exclude<T, null | undefined>

type MaybeName = string | undefined | null
type Name = NonMaybeType<MaybeName>

let a: MaybeName = 'Gabriel' // Ok
let b: MaybeName = null // Ok
let c: Name = 'Gabriel' // Ok
let d: Name = null // Error
```

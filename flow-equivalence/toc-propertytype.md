# $PropertyType&lt;T, k&gt;

## Description

In Flow, a [$PropertyType<T, k>](https://flow.org/en/docs/types/utilities/#toc-propertytype) is the type at a given key `k`. As of Flow v0.36.0, `k` must be a literal string.

Flow Exmaple([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQAXAngBwKagAqYE4DOsAdqALygDeiooJAhgLaYBcohy+AliQOYAaWqAZ829AK5MARgSF10DfJhLJ2eIqUQBfANyJEAY1Kd6meADlm4gCQ58sLPjQAVDJgA8G4iQGgA5Iws-gB8FAEAstyGABYMmNCgAFIMhgDWPv76xiSmJOYAgmLsdg5Oru5eBD5+-qKYoeEArAAM2SbIZvA4SipqoKWOBBVYVZq+AYrKqo2U-kmw2CnpmbpAA))
```javascript
// @flow
type Person = {
  name: string,
  age: number,
  parent: Person
};

const newName: $PropertyType<Person, 'name'> = 'Michael Jackson';
const newAge: $PropertyType<Person, 'age'> = 50;
const newParent: $PropertyType<Person, 'parent'> = 'Joe Jackson';
```

Typescript equavalent of `$PropertyType<T, k>`([playground](https://agentcooper.github.io/typescript-play/#code/C4TwDgpgBAChBOBnA9gOygXigbwFBSlQEMBbCALikWHgEtUBzAGnyiIYsIFcSAjBFgTBF4EVMEpwkaXAF8A3LlyhIUAKIAPGkQDGwACrgIAHn1MoAaQB8mS1AhaxAE0RQA1hBDIAZlH1QAfj8AbQsAXShKVAgANwR5KCUdNGpCCAB3ADlSTk1tPUNIYykUVHMAcmIycpsscoBZWh0ACyIIABsoACldN1LyxWTUVOj0gEEOSjz4XQMjYoRSivYIGtsAVgAGQZTgNPSYETEJdS0ZgvmStArhUXE1uq7kaB6dPrQBqAB6L-V4eGQ8CAA))
```typescript
type Person = {
  name: string,
  age: number,
  parent: Person
};

type ExtractType<T, K> = K extends keyof T ? T[K] : never; 

const newName: ExtractType<Person, 'name'> = 'Michael Jackson';
const newAge: ExtractType<Person, 'age'> = 50;
const newParent: ExtractType<Person, 'parent'> = 'Joe Jackson'; // Error
```

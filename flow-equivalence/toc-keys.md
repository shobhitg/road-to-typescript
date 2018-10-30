# $Keys&lt;T&gt;

## Description

In Flow, [$Keys&lt;T&gt;](https://flow.org/en/docs/types/utilities/#toc-keys) is often used as a substitute for a missing enum functionality in javascript.

Typescript provides [enums](https://basarat.gitbooks.io/typescript/content/docs/enums.html) as a first class entity, and provides the `keyof` operator for use cases which don't exactly match enumerations.

Flow example ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQBjWA7AzgF1KgrmlgE4CWAphqALygDeiooAqgMoBcoARE2iVmQBNQLLAEN+GTgBoGoAJIAVDpzljoAT2myAYgCVl2oqLTIynRAF8A3IkRZ1ABzKgAwrALF11UABIA0mTqGAA89k6wkLjuhKQUAHw2KOjYoHyiGhxuHkReNADkink2qJg4aLBOmdGe3nnlTkWgIKB1FWR5qZTlOKKu1TlAA))

```javascript
// @flow
const countries = {
  US: "United States",
  IT: "Italy",
  FR: "France"
};

type Country = $Keys<typeof countries>;

const italy: Country = 'IT';
const nope: Country = 'nope'; // 'nope' is not a Country
```

TypeScript example with an object ([playground](https://agentcooper.github.io/typescript-play/#code/MYewdgzgLgBKCuYoCcCWBTCMC8MDeAUDDAKoDKAXDAEQlipToAmMZUAhoxNQDREwBJACpVqAjgBsAnr34AxAEqi5ydmGDpqBAL4ECUKQAd0MAMIhEKKThgBrdFJAAzGAePO4FpGkx7QkWAZ2aSpzS2RrXAByYSiAbgJ-aBgwEGNQrysbKNTjeJgAegKYHLT0KJhULFTYdjNMiKA))

```typescript
const countries = {
  US: "United States",
  IT: "Italy",
  FR: "France"
}

type Country = keyof typeof countries

const italy: Country = 'IT';
const nope: Country = 'nope'; // 'nope' is not a Country
```

TypeScript example with an enum ([playground](https://agentcooper.github.io/typescript-play/#code/KYOwrgtgBAwg9mEAXATgS2AZygbwFBRQCqAylALxQBERIaSwAJlCUgIYOZUA0BUAkgBUK1fuwA2ATx58AYgCURVWSjYgAxsCp4AvnjxJJAB2CwEyFJJEBrYJLgAzKIZOOziVBkz71cEJiQoejYpAC53CytKAHIhaIBuPF9-QJA4E3D4D0sRaLSTBKgAeiKoPPTgaKDsNMC2CNRJIA))

```typescript
enum Countries {
  US = "United States",
  IT = "Italy",
  FR = "France"
}

type Country = keyof typeof Countries

const italy: Country = 'IT';
const nope: Country = 'nope'; // 'nope' is not a Country
```

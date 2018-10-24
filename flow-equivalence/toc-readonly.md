# $ReadOnly&lt;T&gt;

## Description

[$ReadOnly&lt;T&gt;](https://flow.org/en/docs/types/utilities/#toc-readonly) is a type that represents the read-only version of a given object type T. A read-only object type is an object type whose keys are all read-only.

Flow example ([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQAXAngBwKagAoCdZ0BnUAXlAG9FRQA7AQwFtMAuUI5PAS1oHMAaaqHq9WdAK6MARpjyCaIUADoViAL4BuRCgzYASpnoATAPK1oqfIRLkAJAeNmLAHivEAfFsSRxtAMbIXLC0oHiYtEayABToBMRsDqbmlnFEAJSUQn7BHJQMzPzCompkoLHWGgpgJgDSoMiwoYZGQuXESiLY5AAsAEyVNINDg4oAongEeKDwABbh09yBfEKKKkrqQA)):

```javascript
// @flow
type Props = {
  name: string,
  age: number,
  // ...
};

type ReadOnlyProps = $ReadOnly<Props>;

function render(props: ReadOnlyProps) {
  const {name, age} = props;  // OK to read
  props.age = 42;             // Error when writing
  // ...
}
```

Typescript equivalent example ([playground](https://agentcooper.github.io/typescript-play/#code/C4TwDgpgBACgTgezAZygXigbwFBSgOwEMBbCALimWDgEt8BzAGlykPvIIFdiAjCOZngD0QqADoJ2AL7ZsoSFABKEQgBMA8vgA2IeElQZlahNpAAePSgB8sgGad8AY2A0TUOBHyr+ACjCIUCiMNU0tkAEosFkcTKiwiUkZWdil0KH99KBEodQBpKGAEdxVVFgyUMTZoDAAWACYs0QBROEQ4KAB3AAtPTtoXBmkgA))

{% hint style="info" %}
[Readonly](https://basarat.gitbooks.io/typescript/docs/types/readonly.html) is an inbuilt feature in typescript.
{% endhint %}

```typescript
type Props = {
  name: string,
  age: number,
  // ...
}

type ReadOnlyProps = Readonly<Props>

function render(props: ReadOnlyProps) {
  const {name, age} = props // OK to read
  props.age = 42 // Error when writing
}
```


# $ReadOnly&lt;T&gt;

## Description

[$ReadOnly&lt;T&gt;](https://flow.org/en/docs/types/utilities/#toc-readonly) is a type that represents the read-only version of a given object type T. A read-only object type is an object type whose keys are all read-only.

Flow example \([playground](https://flow.org/try/#0PTAEAEDMBsHsHcBQAXAngBwKagAoCdZ0BnUAXlAG9FRQA7AQwFtMAuUI5PAS1oHMAaaqHq9WdAK6MARpjyCaIUADoViAL4BuRCgzYASpnoATAPK1oqfIRLkAJAeNmLAHivEAfFsSRxtAMbIXLC0oHiYtEayABToBMRsDqbmlnFEAJSUQn7BHJQMzPzCompkoLHWGgpgJgDSoMiwoYZGQuXESiLY5AAsAEyVNINDg4oAongEeKDwABbh09yBfEKKKkrqQA)\):

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

Typescript equivalent example \([playground](https://www.typescriptlang.org/play/index.html#src=%2F%2F%20Readonly%20is%20now%20an%20inbuilt%20typescript%20feature
%2F%2F%20type%20Readonly<T>%20%3D%20{%20%2F%2F%20%24Readonly<T>
%2F%2F%20%20%20%20%20readonly%20[P%20in%20keyof%20T]%3A%20T[P]
%2F%2F%20}

type%20Props%20%3D%20{
%20%20name%3A%20string%2C
%20%20age%3A%20number%2C
%20%20%2F%2F%20...
}

type%20ReadOnlyProps%20%3D%20Readonly<Props>

function%20render%28props%3A%20ReadOnlyProps%29%20{
%20%20const%20{name%2C%20age}%20%3D%20props%20%2F%2F%20OK%20to%20read
%20%20props.age%20%3D%2042%20%2F%2F%20Error%20when%20writing
})\):

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

